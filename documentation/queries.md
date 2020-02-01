DROP TABLE if exists address_table;
	
	
		SELECT  
		   return_returnheader990x_part_i.ein,
		   return_returnheader990x_part_i.object_id,
	       return_returnheader990x_part_i."RtrnHdr_TxPrdEndDt",
	       return_returnheader990x_part_i."RtrnHdr_TxYr",
	       return_returnheader990x_part_i."BsnssNm_BsnssNmLn1Txt",
	       return_returnheader990x_part_i."BsnssNm_BsnssNmLn2Txt",
	       return_returnheader990x_part_i."BsnssOffcr_PrsnNm",
	       return_returnheader990x_part_i."BsnssOffcr_PrsnTtlTxt",
	       return_returnheader990x_part_i."BsnssOffcr_PhnNm",
	       return_returnheader990x_part_i."BsnssOffcr_EmlAddrssTxt",
	       return_returnheader990x_part_i."BsnssOffcr_SgntrDt",
	       return_returnheader990x_part_i."USAddrss_AddrssLn1Txt",
	       return_returnheader990x_part_i."USAddrss_AddrssLn2Txt",
	       return_returnheader990x_part_i."USAddrss_CtyNm",
	       return_returnheader990x_part_i."USAddrss_SttAbbrvtnCd",
	       return_returnheader990x_part_i."USAddrss_ZIPCd",
	       return_returnheader990x_part_i."FrgnAddrss_AddrssLn1Txt",
			return_returnheader990x_part_i."FrgnAddrss_AddrssLn2Txt",
			return_returnheader990x_part_i."FrgnAddrss_CtyNm",
			return_returnheader990x_part_i."FrgnAddrss_PrvncOrSttNm",
			return_returnheader990x_part_i."FrgnAddrss_CntryCd"      
		INTO address_table
		FROM return_returnheader990x_part_i;

Index it so the queries don't take forever

	DROP INDEX IF EXISTS xx_990_address_oid_ein;
	CREATE INDEX xx_990_address_oid_ein ON address_table (object_id, ein);

### Organization Types

	drop table if exists org_types;
	
	select "Orgnztn501c3Ind", "Orgnztn501cInd", "Orgnztn49471NtPFInd", "Orgnztn527Ind", ein, object_id into org_types from return_part_0;

	insert into org_types select "Orgnztn501c3Ind", "Orgnztn501cInd", "Orgnztn49471NtPFInd", "Orgnztn527Ind", ein, object_id  from return_ez_part_0;

	insert into org_types("Orgnztn501c3Ind", "Orgnztn501cInd", "Orgnztn49471NtPFInd", ein, object_id) select "Orgnztn501c3ExmptPFInd" as "Orgnztn501c3Ind",  "Orgnztn501c3TxblPFInd" as "Orgnztn501cInd", "Orgnztn49471TrtdPFInd" as "Orgnztn49471NtPFInd", ein, object_id from return_pf_part_0;

	DROP INDEX IF EXISTS xx_990_entity_oid_ein;
	CREATE INDEX xx_990_entity_oid_ein ON org_types (object_id, ein);


These fields are used

"Orgnztn501c3Ind": when it's a 501(c)(3)

"Orgnztn501Ind": when it's another type of 501(c) org

"Orgnztn49471NtPFInd": when it's a [4947(a)(1) organization](https://www.irs.gov/charities-non-profits/private-foundations/charitable-trusts) a type of non-exempt trust 

"Orgnztn527Ind": a 527 organization, a non-federal political group e.g. "Swift Vote Veterans for Truth", although many choose to instead file 8871/8872 reports to the IRS. 
	       
	       




Make a temporary table with the stuff to pull from Part VII Section A

	DROP TABLE IF EXISTS tmp_990_employees;
	
	SELECT address_table.*,
       "PrsnNm",
       "TtlTxt",
       "RprtblCmpFrmOrgAmt" as "CmpnstnAmt"
    INTO tmp_990_employees
	FROM return_Frm990PrtVIISctnA
	LEFT JOIN address_table ON return_Frm990PrtVIISctnA.ein = address_table.ein
	AND return_Frm990PrtVIISctnA.object_id=address_table.object_id;






Copy back with:



	\copy tmp_990_employees to '/data2/outputs/990_employees.csv' with csv header;




### 990EZ


	DROP TABLE IF EXISTS tmp_990ez_employees;
	
	SELECT address_table.*,
       "PrsnNm",
       "TtlTxt",
       "CmpnstnAmt" 
	   INTO tmp_990EZ_employees
	   FROM return_EZOffcrDrctrTrstEmpl
		LEFT JOIN address_table ON return_EZOffcrDrctrTrstEmpl.ein = address_table.ein
		AND return_EZOffcrDrctrTrstEmpl.object_id= address_table.object_id;


And then move with: 

 	\copy tmp_990ez_employees to '/data2/outputs/990EZ_employees.csv' with csv header;
 	


### 990PF

Get PF values

	DROP TABLE IF EXISTS tmp_990PF_employees;
	
	SELECT address_table.*,
	       "OffcrDrTrstKyEmpl_PrsnNm" AS "PrsnNm",
	       "OffcrDrTrstKyEmpl_TtlTxt" AS "TtlTxt",
	       "OffcrDrTrstKyEmpl_CmpnstnAmt" AS "CmpnstnAmt" 
	INTO tmp_990PF_employees
	FROM return_PFOffcrDrTrstKyEmpl
	LEFT JOIN address_table ON return_PFOffcrDrTrstKyEmpl.ein = address_table.ein
	AND return_PFOffcrDrTrstKyEmpl.object_id= address_table.object_id;



	
copy it with: 

	\copy tmp_990PF_employees to '/data2/outputs/990PF_employees.csv' with csv header;

