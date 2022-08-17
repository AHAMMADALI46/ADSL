# ADSL
/*ADSL----ADAM subject level information*/
structure: One record/subject
source: SDTM_DM, SDTM_DS, SDTM_MH, SDTM_VS SDTM_EX

Options validvarname=upcase nofmterr missing=' ' msglevel=i;

/*msglevel-i: whenever two or more domains merge together by common variable overlap records information will be pop-up in log winodow by using msglevel=i*/
/*Validvarname=upcase: all the variables in dataset will be uppercase*/
/*Nofmterr: if you have any format it will consider without errors*/

Options validvarname=upcase nofmterr missing=' ' msglevel=i;

Proc sort data=sdtm.dm out=dm; by usubjid; run;

Proc sort data=sdtm.ds out=ds; by usubjid; run;

Proc sort data=sdtm.mh out=mh; by usubjid; run;

Proc sort data=sdtm.vs out=vs; by usubjid; run;

Proc sort data=sdtm.ex out=ex; by usubjid; run;

Proc sort data=sdtm. out=dm; by usubjid; run;

data dm1 (keep=studyid domain usubjid subjid siteid age ageu race dthdtc dthfl  arm armcd ethnic dmdtc dmdy rfstdtc rfendtc actarm actarmcd rfpendtc sexn racen trtp trtpn trta trtan ethnicn dthdy agegrp agegrpn rfstdt rfendt rficdt brthdt dthdt rfpendt);
set dm (drop=domain);
domain="ADSL";
if sex="M" then sexn=1;
else sexn=0;

if race="white" then racen=5;
else if race="Black or African American" then racen=3;;

trtp=armcd;
if armcd="DEX" then trtpn=1;
else trtpn=.;

trta=actarmcd;
if actarmcd="DEX" then trtan=1;
else trtan=.;

if ethnic="Hispanic or Lation" Then ethnicn=1;
else if ethnic="Not hispanic or Latino" then ethnicn=2;
else if ethnic="Unknown" then ethnicn=3;

if age<50 then do; 
agegr="<50 years";
agegrpn=1; end;
else if age >= 50 and age <=65 then do;
agegrp="50 to 65 years";
agegrpn=2; end;
else if age>65 then do;
agegrp=" >65 years";
agrgrpn=3; end;

if rfstdtc ne ' ' then
rfstdt=datepart(input(rfstdtc, anydtdtm.));
if rfendtc ne ' ' then
rfendt=datepart(input(rfendtc, anydtdtm.));
if rfpendtc ne ' ' then
rfpendt=input(rfpendtc, yymmdd10.);
if brthdtc ne ' ' then
brthdt= input(brthdtc, yymmdd10.);
if rficdtc ne ' ' then
brthdt= input(rficdtc, yymmdd10.);
if dthdtc ne ' ' then
dthdt= input(dthdtc, yymmdd10.);

/*Dthdy=(Dthdtc-rfstdtc)+1*/
if nmiss(dthdt, rfstdt)=0 then dthdy=(dthd-rfstdt)+1;
format rfstdt rfendt rficdt brthdt dthdt rfpendt date9.;
run;




