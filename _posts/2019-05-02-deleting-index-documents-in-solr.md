---
layout: post
title: 'Deleting Indexed Documents in Solr using Delta Query'
categories: blog
tags: ['solr', 'deltaQuery']
excerpt: 'The correct way of deleting indexed documents in solr using Delta Query based on certain criteria'
date: May 02, 2019
author: self
---

We were adding new documents using delta query which seems to be an easy task where I just have to write deltaQuery to find out any new documents or any updated documents and then using deltaImportQuery I index those documents. To delete indexed documents in delta query we have to use deletedPkQuery. Now I have to spent some considerable amount of time to do it correctly. I am documenting it so that it would be easier for me to do it next time.

The correct way of using deletedPkQuery is that :

1.  Write SQL Query for getting document's primary key. We will only get those documents which has to be deleted from the index. In the below query myColumn is a primary key and status is 'deleted'.

    ```
    deletedPkQuery="SELECT myColumn as MYCOLUMNID
                    from myTable 
                    WHERE status = 'deleted' 
                    AND modifiedDate &gt; '${dataimporter.last_index_time}'"
    ```

2. The entity should have a pk set i.e. the primary key for the entity as shown below. 

	```
	<entity name="myEntity" 
            pk="MYCOLUMNID"
            query="..."
            deletedPkQuery="SELECT myColumn as MYCOLUMNID
			                from myTable 
			                WHERE status = 'deleted' 
			                AND modifiedDate &gt; '${dataimporter.last_index_time}'"
            deltaImportQuery="..."
            deltaQuery="...">
        <field column="MYCOLUMNID" name="id" />
    </entity>
    ```

3. The uniqueKey in schema.xml should be same as that of pk. When looking at the [solr guide about delta query](https://lucene.apache.org/solr/guide/6_6/uploading-structured-data-store-data-with-the-data-import-handler.html) it says that :

	> The primary key for the entity. It is optional, and required only when using delta-imports. It has no relation to the uniqueKey defined in schema.xml but they can both be the same.

    But for deletedPkQuery to work uniqueKey and pk should be same as shown below.
	
	```
	// field to use to determine and enforce document uniqueness.
	<uniqueKey>id</uniqueKey>
	``` 

After doing these steps, when you run the delta query all the documents whose status is marked as 'deleted' would be deleted from the index.

Thanks for reading.


