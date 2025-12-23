
1.  When you want to do the once again scrapping + pdf processing process again. 
2.  first clear up the data for the s3 folder from the scrapped pdf using below query. 
```
 SELECT * FROM scrapped_pdfs WHERE s3_path = ''
 -- DELETE FROM scrapped_pdfs WHERE s3_path = ''
```

3.  After that Delete data from file_process table using the below query. 
```
 SELECT * FROM file_process_log WHERE s3foldername = ''
 -- DELETE * FROM file_process_log WHERE s3foldername = ''
```
4. Now Delete all file including version ( click on toggle to see the versioning file in s3) from s3 folder in s3 bucket in bronze. 
```

-- We can use thie below s3 cloudshell script to delete desired record from s3 or delete it manually if feasible.

BUCKET="aura-knowledge-hub"
PREFIX="silver/legislation.gov.au/"

# Delete all object versions
aws s3api list-object-versions \
  --bucket "$BUCKET" \
  --prefix "$PREFIX" \
  --output json \
  --query 'Versions[].{Key:Key,VersionId:VersionId}' \
| jq -c '.[]' | while read -r obj; do
  KEY=$(echo $obj | jq -r '.Key')
  VERSION_ID=$(echo $obj | jq -r '.VersionId')
  echo "Deleting version: $KEY (versionId: $VERSION_ID)"
  aws s3api delete-object --bucket "$BUCKET" --key "$KEY" --version-id "$VERSION_ID"
done

# Delete all delete markers
aws s3api list-object-versions \
  --bucket "$BUCKET" \
  --prefix "$PREFIX" \
  --output json \
  --query 'DeleteMarkers[].{Key:Key,VersionId:VersionId}' \
| jq -c '.[]' | while read -r marker; do
  KEY=$(echo $marker | jq -r '.Key')
  VERSION_ID=$(echo $marker | jq -r '.VersionId')
  echo "Deleting delete marker: $KEY (versionId: $VERSION_ID)"
  aws s3api delete-object --bucket "$BUCKET" --key "$KEY" --version-id "$VERSION_ID"
done

#To verify it's empty after execution
aws s3api list-object-versions --bucket "$BUCKET" --prefix "$PREFIX"

```
4. Now run the scrapping process one by one scrapped files will be uploaded to s3 and now  [[Scrapping Work Flow]] will be started automatically.
5. Now to check on going process of pdf of step 5 use this below query. 
```
-- There are total 6 Stages of any particular pdf
1. mark down
2. split processing
3. page range proces
4. summary
5. vector
6. success

SELECT
	STATUS,
	PINECONDE_INDEX_NAME,
	PINECODE_SPARSE_INDEX_NAME,
	F.*
FROM
	FILE_PROCESS_LOG F
	INNER JOIN WEBSITE W ON W.S3FOLDERNAME = F.S3FOLDERNAME
WHERE
	F.S3FOLDERNAME = 'Your s3 foldername'
	
	
-- OR to check the current log based on the status use below query 

SELECT
	STATUS,
	COUNT(*) AS STATUS_COUNT,
	(
		SELECT
			COUNT(*)
		FROM
			FILE_PROCESS_LOG
		WHERE
			S3FOLDERNAME = 'legislation.gov.au'
	) AS total_files
FROM
	FILE_PROCESS_LOG
WHERE
	S3FOLDERNAME = 'legislation.gov.au'
GROUP BY
	STATUS
	ORDER BY
	STATUS_COUNT DESC
```

7. After file process completes to see unsuccessful record to see which are stuck in which state and handle it manually and debug those cases in s3 step up function. 
```
SELECT
	STATUS,
	PINECONE_INDEX_NAME,
	PINECONE_SPARSE_INDEX_NAME,
	F.*
FROM
	FILE_PROCESS_LOG F
	INNER JOIN WEBSITES W ON W.S3FOLDERNAME = F.S3FOLDERNAME
WHERE
	F.S3FOLDERNAME = 'your s3 foldername'
	AND STATUS != 'Success'
```

8. After completing all process now its time to upload this file hash to the pinecone for that we will call aura api which will upload the files to pinecone database then after swagger URL is this https://aura-knowledge-hub.aussizzdesk.com/app1/swagger and we will call the `pinecone/upload-all` end point. this endpoint will execute this query below

```
select status,pinecone_index_name,pinecone_sparse_index_name,f.* from file_process_log f
inner join websites w on w.s3foldername=f.s3foldername
where upload_pinecone is null and status='Success'
```

9.  After uploading data to Pinecone, use the query below to identify which index needs a file hash. Then, call the Aura API’s `file-hash` endpoint with that Pinecone index name to get the file hash and share it with Dhaval Mojidra. (Currently manual; will be automated in step 8.)
```
SELECT DISTINCT
	PINECONE_INDEX_NAME
FROM
	FILE_PROCESS_LOG F
	INNER JOIN WEBSITES W ON W.S3FOLDERNAME = F.S3FOLDERNAME
WHERE
	UPLOAD_PINECONE IS NULL
	AND STATUS ='Success'
```