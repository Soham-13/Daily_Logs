

1.  https://legend.online.immi.gov.au/Pages/AllStacksMigration.aspx first we get to know wheather the new version of the legend(arp) is released or not daily. manually.
2. we have seperate projects for all act, regulations, policy code will be same but the links will be categorized. 
3. e.g. for act we have new version of 03-10-2025 so we will insert that act link in the all_stacklinks.csv for the respective act project we will make the new link based on the previous links. same way we have to do for policy and regulations. ![[Pasted image 20251006124034.png]]
4.  after this when we run out python script it will open the link shown above and first we will insert all the links ands sublink in the left side table of main link in act_latest_version_date.csv
5. then we will open all the in the e.g. act_latest_version_date.csv and we will directly click the ctrl + P and simply save the pdf in to the pdf folder of respective ARP folders. 
6. and while making the pdf we will find any links if the links from the pdf is included in the allowed domain if yes so we will append that link in the e.g. act_latest_version_date.csv file.
7. lets say for crawling one link we found another 10 links from that 5 links is legend link so it will append in the act_latest_version.csv file and 3 links is for comlaw and lesiglation so it will also appended in the same csv and also for this link in the lesiglation folder we will also create csv which will have all the comlaw and lesiglation domain link stored in  it these are the link which are to be actually scrapped other than this are just crawled links. 
8. this process is same for regualtions and policy. 