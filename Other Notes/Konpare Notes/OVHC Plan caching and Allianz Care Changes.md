Branches: 
1. KONPARE CRM : soham_misc_branch(done), KNPR-4445-change-allianz-to-allianz-care-crm(done)
2. KONPARE API : aia_nib_plan_caching(done), KNPR-4445-change-allianz-to-allianz-care-api(done)
3. KONPARE WHITELABEL:  changes_visa_type(done), KNPR-4445-change-allianz-to-allianz-care(done), oshc_plan_price_issue(done)
4. AUSSIZZ CRM UI: konpare(done), allianztoallianzcarekrishachanges(done)
5. AUSSIZZ PRODUCT:  konpare(done)
6. KONDESK CRM UI: kondesk_konpare_osch_ovhc_policydatasync

---------------------------  DATABASE  SP and Update Script --------------------------

------------------  KONPARE (done) --------------
upsert_ovhc_plan_helper_sp_POA
nib_plan_upsert_scheduler_sp_POA
aia_plan_upsert_scheduler_sp_POA
get_ovhc_plans
nibapplicantlead
getRefundAnalyticsData
getOshcProviderWiseDailyData
getOshcProviderWiseMonthlyData
getAgentOshcProviderWiseMonthlyData
getUniversity
ovhcinquiry_Insert_SP
ALTER TABLE tblovhcprovidermaster
ADD deschtmlstring NVARCHAR(MAX);

ALTER TABLE tblovhcplans
ADD yearlyamount DECIMAL(18,2);

alter table tblovhcplans 
add broserurl nvarchar(max) default NULL

update tblovhcprovidermaster set 
logourl = N'https://konze.com/wp-content/uploads/2022/09/nib.png', 
topfeatures = N'8501 Compliant Coverage | Providing smart solutions by nib App |  Cover for doctor (GP) consultations |  Access to in-hospital cover'
where id = 3;


update tblovhcprovidermaster set 
logourl = N'https://konze.konpare.online/images/aia-logo.svg', 
topfeatures = N'8501 Compliant Coverage | Award winning insurance | 24/7 Health line | Cover for emergencies'
where id = 8;

UPDATE tblovhcprovidermaster
SET deschtmlstring = 
N'<div style="color: #0080F8; font-size: 18px; font-weight: 500; font-family: ''Hanken Grotesk'', sans-serif;">
    Get 6 Weeks Free. <a href="https://health.aia.com.au/international/documents/aia-health-ovhc-june-2025-offer.pdf"
        target="_blank" style="color: #7D7D7D; padding-left: 5px; font-size: 14px;">T&amp;Cs Apply</a>
</div>' where id = 8; 


update tblovhcplans set tagline = 'Hospital Costs | Emergency Ambulance', offer = 'Includes a $500 excess' where planname = 'Budget Visitor Cover'
update tblovhcplans set tagline = 'Get 60% back on each visit for Dental, Optical and Physio', offer = 'Includes a $500 excess' where planname = 'Budget Visitor Cover Extra'
update tblovhcplans set tagline = 'Pharmaceutical Prescriptions ($50 per item limit & co-pay apply) | Doctor (GP) (100% MBS) | Specialist/Surgeon - 85% MBS', offer = 'Includes a $500 excess' where planname = 'Standard Visitor Cover'
update tblovhcplans set tagline = 'Unlimited preventative dental | General/Major dental: $600 /yr | Optical: $250 | Physiotherapy: $400', offer = 'Includes a $500 excess' where planname = 'Standard Visitor Cover Extra'
update tblovhcplans set tagline = 'Specialist/Surgeon - 100% MBS | Outpatient pregnancy', offer = 'Includes a $500 excess' where planname = 'Advantage Visitor Cover Extra'
update tblovhcplans set tagline = 'Emergency ambulance | Repatriation or funeral expenses', offer = 'Choice of $500 excess' where providerplancode in ('BC7WA','BC5WA')
update tblovhcplans set tagline = '100% of the MBS for a range of out-of-hospital medical services | Travel and accommodation', offer = 'Choice of $750 excess' where providerplancode in ('SC7WA','SC5WA')
update tblovhcprovidermaster set description = 'Health insurance that is right for you and meets your working visa requirements.' where id in (2,3)
update tblovhcprovidermaster set description = 'We offer a number of insurance solutions for your situation. Here is a summary of cover options.' where id in (1)
update tblovhcprovidermaster set description = 'Our Overseas Visitor health insurance means you or a visiting family member can enjoy your stay.' where id in (5)
update tblovhcprovidermaster set description = null where id in (6)
update tblovhcprovidermaster set description = N'<b>Get 6 Weeks Free.</b> Affordable health cover that meets your visa requirements and protects you while you are in Australia.' where id in (8)

--update tblovhcplans set broserurl = 'https://payment.ovhcallianzassistance.com.au/Content/files/PolicyDocuments/imp2593-ovhc-policy-visitors.pdf' where provider = 1
--update tblovhcplans set broserurl = 'https://www.nib.com.au/docs/budget-visitor-cover-factsheet' where provider  = 3 and planname in ('Budget Visitor Cover', 'Budget Visitor Cover Extra')
--update tblovhcplans set broserurl = 'https://www.nib.com.au/docs/standard-visitor-cover-factsheet' where provider  = 3 and planname in ('Standard Visitor Cover', 'Standard Visitor Cover Extra')
--update tblovhcplans set broserurl = 'https://www.nib.com.au/docs/advantage-visitor-cover-factsheet' where provider  = 3 and planname in ('Advantage Visitor Cover Extra')
--update tblovhcplans set broserurl = 'https://health.aia.com.au/international/documents/aia-overseas-visitors-health-cover-standard-cover-fact-sheet.pdf' where provider = 8 and planname in ('Standard Cover Excess $750', 'Standard Cover Excess $500')
--update tblovhcplans set broserurl = 'https://health.aia.com.au/international/documents/aia-overseas-visitors-health-cover-base-cover-fact-sheet.pdf' where provider = 8 and planname in ('Base Cover Excess $750', 'Base Cover Excess $500')
--update tblovhcplans set broserurl = 'https://konpare.online/uploadedfiles/broser/medibank-overseas-visitors-starter-hospital-and-medical (1).pdf' where planname = 'Overseas Visitor Starter Hospital and Medical' and provider = 5
--update tblovhcplans set broserurl = 'https://konpare.online/uploadedfiles/broser/medibank-overseas-visitors-everyday-hospital-and-medical.pdf' where planname = 'Overseas Visitors Everyday Hospital and Medical' and provider = 5
--update tblovhcplans set broserurl = 'https://konpare.online/uploadedfiles/broser/medibank-overseas-visitors-starter-hospital-and-medical (1).pdf' where planname = 'Overseas Visitors Starter Hospital and Medical' and provider = 5
--update tblovhcplans set broserurl = 'https://konpare.online/uploadedfiles/broser/Overseas Workers Advanced Hospital and Medical 400.pdf' where planname = 'Overseas Workers Advanced Hospital and Medical' and provider = 5 
--update tblovhcplans set broserurl = 'https://konpare.online/uploadedfiles/broser/medibank-overseas-workers-base-hospital (1).pdf' where planname = 'Overseas Workers Base Hospital' and provider = 5 
--update tblovhcplans set broserurl = 'https://konpare.online/uploadedfiles/broser/medibank-overseas-workers-premium-hospital-medical-and-extras (1).pdf' where planname = 'Overseas Workers Premium Hospital, Medical and Extras' and provider = 5
--update tblovhcplans set broserurl = 'https://konpare.online/uploadedfiles/broser/medibank-overseas-workers-standard-hospital-and-medical (1).pdf' where planname = 'Overseas Workers Standard Hospital and Medical' and provider = 5
--update tblovhcplans set broserurl = 'https://bupaanzstdhtauspub01.blob.core.windows.net/productfiles/Essential_50_Visitors_Cover_NSW_ACT_S_20210124_151808.pdf' where planname = 'Essential 50 Visitors Cover' and provider = 2
--update tblovhcplans set broserurl = 'https://bupaanzstdhtauspub01.blob.core.windows.net/productfiles/Essential_Lite_Visitors_Cover_NSW_ACT_S_20210119_195110.pdf' where planname = 'Essential Lite Visitors Cover' and provider = 2
--update tblovhcplans set broserurl = 'https://bupaanzstdhtauspub01.blob.core.windows.net/productfiles/Essential_Visitors_Cover_NSW_ACT_S_20210119_195057.pdf' where planname = 'Essential Visitors Cover' and provider = 2
--update tblovhcplans set broserurl = 'https://konpare.online/uploadedfiles/broser/Explorer_Visitors_Cover_NSW_ACT_C_20250205_130034.pdf' where planname = 'Explorer Visitors Cover' and provider = 2 
--update tblovhcplans set broserurl = 'https://bupaanzstdhtauspub01.blob.core.windows.net/productfiles/Premium_Visitors_Cover_with_Excess_NSW_ACT_S_20210119_195056.pdf' where planname = 'Explorer Visitors Cover with Freedom 50 Extras' and provider = 2 
--update tblovhcplans set broserurl = 'https://bupaanzstdhtauspub01.blob.core.windows.net/productfiles/Mid_60_Visitors_Cover_NSW_ACT_S_20210124_152006.pdf' where planname = 'Mid 60 Visitors Cover' and provider = 2
--update tblovhcplans set  planid = 5
--where planname = 'Overseas Visitor Starter Hospital and Medical' and provider = 5
--update tblovhcplans set  planid = 6
--where planname = 'Overseas Visitors Everyday Hospital and Medical' and provider = 5
--update tblovhcplans set  planid = 5 -- confusion
--where planname = 'Overseas Visitors Starter Hospital and Medical' and provider = 5
--update tblovhcplans set planid = 3
--where planname = 'Overseas Workers Advanced Hospital and Medical' and provider = 5 
--update tblovhcplans set planid = 1
--where planname = 'Overseas Workers Base Hospital' and provider = 5 
--update tblovhcplans set planid = 4
--where planname = 'Overseas Workers Premium Hospital, Medical and Extras' and provider = 5
--update tblovhcplans set planid = 2
--where planname = 'Overseas Workers Standard Hospital and Medical' and provider = 5


update tblovhcinquiry set hospitalexcess = '500', recurringtype = 'monthly' where id > 1



--update tblovhcprovidermaster set name ='Allianz Care' where name = 'Allianz'
--update tblagentovhcprovidermaster set name ='Allianz Care' where name = 'Allianz'
--update tblagentcommissionmaster set name ='Allianz Care' where name = 'Allianz'
--update tblagentinsurencecommission set name ='Allianz Care' where name = 'Allianz'
--update tblInstitution_provider set Provider ='Allianz Care' where Provider = 'Allianz' 
--update agrp_pagecontent set link_name ='allianz care' where link_name = 'allianz' 
--update agrp_pagecontent set h1_tag ='ALLIANZ CARE' where h1_tag = 'ALLIANZ' 
--update tblagentcommissionmaster set description ='Allianz Care' where description = 'Allianz'
--update tblagentinsurencecommission set description ='Allianz Care' where description = 'Allianz'
--update tblocrpolicycertificatelog set type = 'allianz care' where id = 2
--UPDATE tblagentagreement 
--SET commmissiondetails = REPLACE(commmissiondetails, 'Allianz :', 'Allianz Care')
--WHERE commmissiondetails LIKE '%Allianz%';





----------- AUSSIZZ (done) ----------------------


update tblkonpareinsurance set provider = 'Allianz Care' where provider like '%Allianz%'
update tblovhclogs set providername = 'Allianz Care' where providername like '%Allianz%'




-------------------------------- CODE CHANGES  (done) -------------------------------------------------
KONPARE CRM: 
D:\Konpare\konpare_ui\CRM_UI\Areas\Ovhcpolicy\Views\Ovhc\PoliciesPurchased.cshtml
D:\Konpare\konpare_ui\CRM_UI\Areas\Ovhcpolicy\Views\Ovhc\Index.cshtml
D:\KONPARE\konpare_ui\CRM_UI\Areas\Analytics\AngularJS\OshcAnalyticsController.js
D:\KONPARE\konpare_ui\CRM_UI\Areas\Analytics\AngularJS\RefundAnalyticsController.js
D:\KONPARE\konpare_ui\CRM_UI\Areas\Analytics\Views\OshcAnalytics\OshcReport.cshtml
D:\KONPARE\konpare_ui\CRM_UI\Areas\Company\AngularJS\CommonDashboardController.js
D:\KONPARE\konpare_ui\CRM_UI\Areas\Inquiry\AngularJS\InquiryController.js
D:\KONPARE\konpare_ui\CRM_UI\Areas\OCR\Views\Ocrreport\Index.cshtml
D:\KONPARE\konpare_ui\CRM_UI\Areas\Ovhcpolicy\AngularJS\OvhcController.js
D:\KONPARE\konpare_ui\CRM_UI\Areas\Purchasepolicy\AngularJS\AutomationController.js
D:\KONPARE\konpare_ui\CRM_UI\Areas\Report\AngularJS\AgentinsightController.js
D:\KONPARE\konpare_ui\CRM_UI\Areas\Report\AngularJS\AgentPerformanceReport.js
D:\KONPARE\konpare_ui\CRM_UI\Areas\Report\Views\Report\CommissionHistoryReport.cshtml
D:\KONPARE\konpare_ui\CRM_UI\Areas\Report\Views\Report\_agentmontlyreport.cshtml
D:\KONPARE\konpare_ui\CRM_UI\Areas\Report\Views\Report\_purchasedetails.cshtml


  

AUSSIZZ CRM: 
D:\Aussizz\aussizzdesk_crm_ui\CRM_UI\Areas\Product\AngularJS\OVHCInsuranceController.js
D:\Aussizz\aussizzdesk_crm_ui\CRM_UI\Areas\Product\Views\OVHCInsurance\_OVHCInsurancePartial.cshtml
D:\Aussizz\aussizzdesk_crm_ui\CRM_UI\Areas\Product\AngularJS\InsuranceController.js
D:\Aussizz\aussizzdesk_crm_ui\CRM_UI\Areas\Product\AngularJS\InsuranceOrderInvoiceController.js
D:\Aussizz\aussizzdesk_crm_ui\CRM_UI\Areas\Product\Views\Insurance\_insurance.cshtml


WHITELABEL: 
D:\Konpare\konpare_whitelabel\ovhc.aspx
D:\Konpare\konpare_whitelabel\ovhc.aspx.cs
D:\Konpare\konpare_whitelabel\ovhccompare.aspx
D:\Konpare\konpare_whitelabel\ovhccompare.aspx.cs
D:\Konpare\konpare_whitelabel\ovhcnibcompare.aspx
D:\Konpare\konpare_whitelabel\ovhcnibcompare.aspx.cs
D:\Konpare\konpare_whitelabel\ovhcsum-aia-popup.aspx
D:\Konpare\konpare_whitelabel\ovhcsum-aia-popup.aspx.cs
D:\Konpare\konpare_whitelabel\ovhcsum-popup.aspx
D:\Konpare\konpare_whitelabel\ovhcsum-popup.aspx.cs
D:\Konpare\konpare_whitelabel\main.master
D:\Konpare\konpare_whitelabel\ovhc-thankyou.aspx
D:\KONPARE\konpare_whitelabel\index.aspx
D:\KONPARE\konpare_whitelabel\index.aspx.cs
D:\KONPARE\konpare_whitelabel\screen2.aspx
D:\KONPARE\konpare_whitelabel\screen3.aspx
D:\KONPARE\konpare_whitelabel\screen3.aspx.cs
D:\KONPARE\konpare_whitelabel\oshc_extension.aspx
D:\KONPARE\konpare_whitelabel\oshc_extension.aspx.cs
D:\KONPARE\konpare_whitelabel\nib_oshc.aspx
D:\KONPARE\konpare_whitelabel\nib_oshc.aspx.cs
D:\KONPARE\konpare_whitelabel\payinr.aspx.cs
D:\KONPARE\konpare_whitelabel\siteContent\oshc-features.json
D:\KONPARE\konpare_whitelabel\siteContent\oshc-features-vi-VN.json
D:\KONPARE\konpare_whitelabel\siteContent\oshc-features-zh-CN.json
D:\KONPARE\konpare_whitelabel\App_Code\automationAPiCall.cs
D:\KONPARE\konpare_whitelabel\App_Code\CommonLib.cs
D:\Konpare\konpare_whitelabel\App_Code\BundleConfig.cs
D:\Konpare\konpare_whitelabel\App_Code\StudentDetails.cs

KONDESK CRM: 
D:\Projects\kondesk_crm_ui\CRM_UI\Areas\Product\AngularJS\InsuranceController.js
D:\Projects\kondesk_crm_ui\CRM_UI\Areas\Product\AngularJS\InsuranceOrderInvoiceController.js
D:\Projects\kondesk_crm_ui\CRM_UI\Areas\Product\AngularJS\OVHCInsuranceController.js
D:\Projects\kondesk_crm_ui\CRM_UI\Areas\Product\Views\OVHCInsurance\_OVHCInsurancePartial.cshtml

