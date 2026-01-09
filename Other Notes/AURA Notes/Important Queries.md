
**when the data exist in the FILE_PROCESS_LOG table but not in the SCRAPPED_PDFS so use below insert query to insert the missing records in the scrapped_pdfs**

```
NSERT INTO scrapped_pdfs (  
    scrape_link_id,  
    file_name,  
    s3_path,  
    base_filename,  
    file_hash  
)  
SELECT  
    CAST(  
        substring(fpl.file_name FROM '_([0-9]+)\.pdf$')  
        AS INTEGER  
    )                           AS scrape_link_id,  
    fpl.file_name,  
    'bronze/study.csu.edu.au'    AS s3_path,  
    fpl.file_name               AS base_file_name,  
    fpl.file_hash  
FROM file_process_log fpl  
LEFT JOIN scrapped_pdfs sp  
       ON sp.file_name = fpl.file_name  
WHERE sp.id IS NULL  
  AND fpl.s3foldername = 'utas.edu.au';
```

