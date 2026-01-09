  **Get OSHC and OVHC Client Direct Uploaded Policy Provider Wise**
```
select top 100 l.leadcode from tblovhclogs ol 
left join tbllead l on l.leadid = ol.leadidf
where lower(ol.providername) like '%cbhs%' and ISNULL(ol.ispaid,0) = 1 and ISNULL(ol.purchase_type, '') = 'direct' order by ol.id desc

select top 100 l.leadcode from tblkonpareinsurance ki
left join tbllead l on ki.leadid = l.leadid
where lower(ki.provider) like '%medibank%' and ISNULL(ki.ispayment,0) = 1 and ISNULL(ki.purchase_type,'') = 'direct' order by  ki.id desc
```

**Get branch name and its whitelable Details ( Aussizz)**

```
select tc.companyname, ki.whitelabel_url from konpare_integration ki 
left join tblcompany tc on tc.companyid = ki.branch_id
where ISNULL(ki.sec_key,'') != ''  and ISNULL(ki.whitelabel_url,'') != ''  order by ki.konpare_integration_id desc
```



