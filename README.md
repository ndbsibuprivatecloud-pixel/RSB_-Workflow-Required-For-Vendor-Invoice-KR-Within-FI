# RSB_-Workflow-Required-For-Vendor-Invoice-KR-Within-FI
Workflow Required For Vendor Invoice(KR) Within FI

ZFI_051_ACCDOCAPR_T1 Package 

Service Bindings 

@EndUserText.label: 'Service definition for ZC_FIAMOUNTLIMIT'
define service ZUI_FIAMOUNTLIMIT_O4 {
  expose ZC_FIAMOUNTLIMIT as AMOUNTLIMIT;
}
********************************************************************************************

@EndUserText.label: 'Service definition for ZC_FIDOCUSERS'
define service ZUI_FIDOCUSERS_O4 {
  expose ZC_FIDOCUSERS as Fidocmaint;
}
************************************************************************************************

@EndUserText.label: 'Service definition for ZC_FIWFLOG'
define service ZUI_FIWFLOG_O4 {
  expose ZC_FIWFLOG;
}
*******************************************************************************************

Behaviour Definitions 

projection;
strict ( 2 );
use draft;

define behavior for ZC_FIAMOUNTLIMIT alias AMOUNTLIMIT
use etag

{
  use create;
  use update;
  use delete;

  use action Edit;
  use action Activate;
  use action Discard;
  use action Resume;
  use action Prepare;
******************************************************************************************

projection;
strict ( 2 );
use draft;

define behavior for ZC_FIDOCUSERS alias Fidocmaint
use etag

{
  use create;
  use update;
  use delete;

  use action Edit;
  use action Activate;
  use action Discard;
  use action Resume;
  use action Prepare;
}
**********************************************************************************************************

managed implementation in class ZBP_R_FIAMOUNTLIMIT unique;
strict ( 2 );
with draft;

define behavior for ZI_FIAMOUNTLIMIT alias AMOUNTLIMIT
persistent table zfiamountlimit
draft table zfiamountlimit_d
etag master LocalInstanceLastChangedAt
lock master total etag LastChangedAt
authorization master ( global )

{
  field ( numbering : managed )
  uuid;

  field ( mandatory : create )
  Application,
//  Fromamount,
  Fromdate,
  Todate;
//  Toamount;


  field ( readonly )
  uuid,
  LastChangedAt,
  LastChangedBy,
  LocalInstanceLastChangedAt;

  field ( readonly : update )
  Application;


 validation MandateVales on save { create; update; field Application, Fromdate,todate,
  fromamount,toamount; }

  create;
  update;
  delete;

  draft action Edit;
  draft action Activate optimized;
  draft action Discard;
  draft action Resume;
  draft determine action Prepare;

  mapping for zfiamountlimit
    {
      uuid                       = uuid;
      Application                = application;
      Fromdate                   = fromdate;
      Todate                     = todate;
      Uom                        = uom;
      Fromamount                 = fromamount;
      Toamount                   = toamount;
      wflevel                    = wflevel;
      LastChangedBy              = last_changed_by;
      LastChangedAt              = last_changed_at;
      LocalInstanceLastChangedAt = local_instance_last_changed_at;
    }
}
**************************************************************************************************************

managed implementation in class ZBP_R_FIDOCUSERS unique;
strict ( 2 );
with draft;

define behavior for ZI_FIDOCUSERS alias Fidocmaint
persistent table zfidocusers
draft table zfidocusers_d
etag master LocalInstanceLastChangedAt
lock master total etag LastChangedAt
authorization master ( global )

{
  field ( mandatory : create )
  application;

  // Documenttype;

  field ( readonly )
  CreatedAt,
  CreatedBy,
  LastChangedAt,
  LastChangedBy,
  LocalInstanceLastChangedAt;

  field ( readonly : update )
  //Documenttype;

  application;
  create;
  update;
  delete;

  draft action Edit;
  draft action Activate optimized;
  draft action Discard;
  draft action Resume;
  draft determine action Prepare;

  mapping for zfidocusers
    {

      //Documenttype = documenttype;
      application                = application;
      Userone                    = userone;
      altuserone                 = altuserone;
      Usertwo                    = usertwo;
      altUsertwo                 = altusertwo;
      Userthree                  = userthree;
      altUserthree               = altuserthree;
      Userfour                   = userfour;
      altUserfour                = altuserfour;
      Userfive                   = userfive;
      altUserfive                = altuserfive;
      CreatedBy                  = created_by;
      CreatedAt                  = created_at;
      LastChangedBy              = last_changed_by;
      LastChangedAt              = last_changed_at;
      LocalInstanceLastChangedAt = local_instance_last_changed_at;
    }
}
*************************************************************************************************************

Data Definitions 

@AccessControl.authorizationCheck: #CHECK
@Metadata.allowExtensions: true
@EndUserText.label: 'Projection View for ZI_FIAMOUNTLIMIT'
@ObjectModel.semanticKey: [ 'Application' ]
define root view entity ZC_FIAMOUNTLIMIT
  provider contract transactional_query
  as projection on ZI_FIAMOUNTLIMIT
{
  key Application,
  key uuid,
      @EndUserText.label : 'From Date'
      Fromdate,
      @EndUserText.label : 'To Date'
      Todate,
      @EndUserText.label : 'From Amount'
      Fromamount,
      @EndUserText.label : 'To Amount'
      Toamount,
      Uom,
      @EndUserText.label : 'Level'
      wflevel,
      LocalInstanceLastChangedAt

}
**************************************************************************************************************

@AccessControl.authorizationCheck: #NOT_REQUIRED
@Metadata.allowExtensions: true
@EndUserText.label: 'Projection View for ZI_FIDOCUSERS'
@ObjectModel.semanticKey: [ 'application' ]
define root view entity ZC_FIDOCUSERS
  provider contract transactional_query
  as projection on ZI_FIDOCUSERS
{
      //  key application,
      @EndUserText.label : 'Application Name'
      @Consumption.valueHelpDefinition: [{ entity: { name: 'ZI_ApplicationVH', element: 'Description' }, useForValidation: true }]
  key application,
      Userone,
      altuserone,
      Usertwo,
      altUsertwo,
      Userthree,
      altUserthree,
      Userfour,
      altUserfour,
      Userfive,
      altUserfive,
      LocalInstanceLastChangedAt

}
****************************************************************************************************************

@AccessControl.authorizationCheck: #NOT_REQUIRED
@Metadata.allowExtensions: true
@EndUserText.label: 'Projection View for ZI_FIWFLOG'
@ObjectModel.semanticKey: [ 'WorkflowID', 'Documentno', 'Application' ]
define root view entity ZC_FIWFLOG
  provider contract transactional_query
  as projection on ZI_FIWFLOG
{
  key WorkflowID,
  key Documentno,
  key Application,
      Doccreatedby,
      @Semantics.businessDate.at: true
      Doccreatedon,
      Wflevel,
      Status,
      Userone,
      altuserone,
      @Semantics.businessDate.at: true
      Approvaldateu1,
      Remarksu1,
      status2,
      Usertwo,
      altusertwo,
      @Semantics.businessDate.at: true
      Approvaldateu2,
      Remarksu2,
      status3,
      Userthree,
      altuserthree,
      @Semantics.businessDate.at: true
      Approvaldateu3,
      Remarksu3,
      status4,
      Userfour,
      altuserfour,
      @Semantics.businessDate.at: true
      Approvaldateu4,
      Remarksu4,
      status5,
      Userfive,
      altuserfive,
      @Semantics.businessDate.at: true
      Approvaldateu5,
      Remarksu5,
      manualflag,
      LocalInstanceLastChangedAt

}
********************************************************************************************************************

@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Value Help for Application Name''s'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
@ObjectModel.dataCategory: #VALUE_HELP
@ObjectModel.representativeKey: 'Description'
@ObjectModel.supportedCapabilities: [#SQL_DATA_SOURCE,
                                     #CDS_MODELING_DATA_SOURCE,
                                     #CDS_MODELING_ASSOCIATION_TARGET,
                                     #VALUE_HELP_PROVIDER,
                                     #SEARCHABLE_ENTITY]
@ObjectModel.modelingPattern:#NONE
@Search.searchable: true
@Consumption.ranked: true
define view entity ZI_AppVH
  as select from ZI_ApplicationVH
{
      @ObjectModel.text.element: [ 'tcode' ]
      @Search.defaultSearchElement: true
      @Search.fuzzinessThreshold: 0.8
      @Search.ranking: #HIGH
  key Description,
      @Semantics.text: true
      @Search.defaultSearchElement: true
      @Search.fuzzinessThreshold: 0.8
      @Search.ranking: #LOW
      tcode
}
******************************************************************************************************************

@AccessControl.authorizationCheck: #CHECK
@EndUserText.label: '##GENERATED Amount limit'
define root view entity ZI_FIAMOUNTLIMIT
  as select from zfiamountlimit as AMOUNTLIMIT
{
  key  application                    as Application,
  key  uuid                           as uuid,
       @EndUserText.label : 'From Date'
       fromdate                       as Fromdate,
       @EndUserText.label : 'To Date'
       todate                         as Todate,
       @Semantics.amount.currencyCode: 'Uom'
       @EndUserText.label : 'From Amount'
       fromamount                     as Fromamount,
       @Semantics.amount.currencyCode: 'Uom'
       @EndUserText.label : 'To Amount'
       toamount                       as Toamount,
       uom                            as Uom,
       @EndUserText.label : 'Level'
       wflevel                        as wflevel,
       @Semantics.user.lastChangedBy: true
       last_changed_by                as LastChangedBy,
       @Semantics.systemDateTime.lastChangedAt: true
       last_changed_at                as LastChangedAt,
       @Semantics.systemDateTime.localInstanceLastChangedAt: true
       local_instance_last_changed_at as LocalInstanceLastChangedAt

}
***********************************************************************************************************************************

@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: '##GENERATED Maint User for Approval'
define root view entity ZI_FIDOCUSERS
  as select from zfidocusers as Fidocmaint
{
      @EndUserText.label : 'Application Name'
  key application                    as application,
      userone                        as Userone,
      altuserone                     as altuserone,
      usertwo                        as Usertwo,
      altusertwo                     as altUsertwo,
      userthree                      as Userthree,
      altuserthree                   as altUserthree,
      userfour                       as Userfour,
      altuserfour                    as altUserfour,
      userfive                       as Userfive,
      altuserfive                    as altUserfive,
      @Semantics.user.createdBy: true
      created_by                     as CreatedBy,
      @Semantics.systemDateTime.createdAt: true
      created_at                     as CreatedAt,
      @Semantics.user.lastChangedBy: true
      last_changed_by                as LastChangedBy,
      @Semantics.systemDateTime.lastChangedAt: true
      last_changed_at                as LastChangedAt,
      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      local_instance_last_changed_at as LocalInstanceLastChangedAt

}
*********************************************************************************************************************************************

@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: '##GENERATED ZFIWFLOG'
define root view entity ZI_FIWFLOG
  as select from zfiwflog
{
      @EndUserText.label : 'Workflow ID'
  key workflowid                     as WorkflowID,
  key documentno                     as Documentno,
      @EndUserText.label : 'Application Name'
  key application                    as Application,
      doccreatedby                   as Doccreatedby,
      @Semantics.businessDate.at: true
      doccreatedon                   as Doccreatedon,
      wflevel                        as Wflevel,
      status                         as Status,
      userone                        as Userone,
      altuserone                     as altuserone,
      @Semantics.businessDate.at: true
      approvaldateu1                 as Approvaldateu1,
      remarksu1                      as Remarksu1,
      status2                        as status2,
      usertwo                        as Usertwo,
      altusertwo                     as altusertwo,
      @Semantics.businessDate.at: true
      approvaldateu2                 as Approvaldateu2,
      remarksu2                      as Remarksu2,
      status3                        as status3,
      userthree                      as Userthree,
      altuserthree                   as altuserthree,
      @Semantics.businessDate.at: true
      approvaldateu3                 as Approvaldateu3,
      remarksu3                      as Remarksu3,
      status4                        as status4,
      userfour                       as Userfour,
      altuserfour                    as altuserfour,
      @Semantics.businessDate.at: true
      approvaldateu4                 as Approvaldateu4,
      remarksu4                      as Remarksu4,
      status5                        as status5,
      userfive                       as Userfive,
      altuserfive                    as altuserfive,
      @Semantics.businessDate.at: true
      approvaldateu5                 as Approvaldateu5,
      remarksu5                      as Remarksu5,
      manualflag                     as manualflag,
      @Semantics.user.lastChangedBy: true
      last_changed_by                as LastChangedBy,
      @Semantics.systemDateTime.lastChangedAt: true
      last_changed_at                as LastChangedAt,
      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      local_instance_last_changed_at as LocalInstanceLastChangedAt

}

Metadata Extensions

@Metadata.layer: #CORE
@UI: {
  headerInfo: {
    typeName: 'AMOUNTLIMIT',
    typeNamePlural: 'AMOUNTLIMITs'
  }
}
annotate view ZC_FIAMOUNTLIMIT with
{
  @UI.facet: [ {
    id: 'idIdentification',
    type: #IDENTIFICATION_REFERENCE,
    label: 'AMOUNTLIMIT',
    position: 10
  } ]
  @UI.lineItem: [ {
    position: 10 ,
    importance: #MEDIUM,
    label: 'Application'
  } ]
  @UI.identification: [ {
    position: 10 ,
    label: 'Application'
  } ]
  @EndUserText.label : 'Application'
@Consumption.valueHelpDefinition: [{ entity: { name: 'ZI_AppVH', element: 'Description' } }]
  Application;

  @UI.lineItem: [ {
    position: 20 ,
    importance: #MEDIUM,
    label: 'From Date'
  } ]
  @UI.identification: [ {
    position: 20 ,
    label: 'From Date'
  } ]
  Fromdate;

  @UI.lineItem: [ {
    position: 30 ,
    importance: #MEDIUM,
    label: 'To Date'
  } ]
  @UI.identification: [ {
    position: 30 ,
    label: 'To Date'
  } ]
  Todate;

  @UI.lineItem: [ {
    position: 40 ,
    importance: #MEDIUM,
    label: 'UOM'
  } ]
  @UI.identification: [ {
    position: 40 ,
    label: 'UOM'
  } ]
  @UI.hidden: true
  Uom;

  @UI.lineItem: [ {
    position: 50 ,
    importance: #MEDIUM,
    label: 'From Amount'
  } ]
  @UI.identification: [ {
    position: 50 ,
    label: 'From Amount'
  } ]
  Fromamount;

  @UI.lineItem: [ {
    position: 60 ,
    importance: #MEDIUM,
    label: 'To Amount'
  } ]
  @UI.identification: [ {
    position: 60 ,
    label: 'To Amount'
  } ]
  Toamount;

  @UI.lineItem: [ {
    position: 70 ,
    importance: #MEDIUM,
    label: 'Level'
  } ]
  @UI.identification: [ {
    position: 70 ,
    label: 'Level'
  } ]
  @EndUserText.label : 'Level'
  wflevel;

  @UI.hidden: true
  LocalInstanceLastChangedAt;
}
*********************************************************************************************************************

@Metadata.layer: #CORE
@UI: {
  headerInfo: {
    typeName: 'Maintain Workflow Approver',
    typeNamePlural: 'Maintain Workflow Approvers'
  }
}
annotate view ZC_FIDOCUSERS with
{
  @UI.facet: [ {
    id: 'idIdentification',
    type: #IDENTIFICATION_REFERENCE,
    label: 'Maintain Workflow Approver',
    position: 10
  } ]
  @UI.lineItem: [ {
    position: 10 ,
    importance: #MEDIUM,
    label: ''
  } ]
  @UI.identification: [ {
    position: 10 ,
    label: 'Application Name'
  } ]
  
 // @Consumption.valueHelpDefinition: [{ entity: { name: 'ZI_ApplicationVH', element: 'Description' }, useForValidation: true }]
  @Consumption.valueHelpDefinition: [{ entity: { name: 'ZI_AppVH', element: 'Description' } }]
 
  application;

  @UI.lineItem: [ {
    position: 20 ,
    importance: #MEDIUM,
    label: 'Approver-1'
  } ]
  @UI.identification: [ {
    position: 20 ,
    label: 'Approver-1'
  } ]
  Userone;

@UI.lineItem: [ {
    position: 30 ,
    importance: #MEDIUM,
    label: 'Alternative Approver-1'
  } ]
  @UI.identification: [ {
    position: 30 ,
    label: 'Alternative Approver-1'
  } ]
  
altuserone;

  @UI.lineItem: [ {
    position: 40 ,
    importance: #MEDIUM,
    label: 'Approver-2'
  } ]
  @UI.identification: [ {
    position: 40 ,
    label: 'Approver-2'
  } ]
  Usertwo;
  
  @UI.lineItem: [ {
    position: 50 ,
    importance: #MEDIUM,
    label: 'Alternative Approver-2'
  } ]
  @UI.identification: [ {
    position: 50 ,
    label: 'Alternative Approver-2'
  } ]
  altUsertwo;

  @UI.lineItem: [ {
    position: 60 ,
    importance: #MEDIUM,
    label: 'Approver-3'
  } ]
  @UI.identification: [ {
    position: 60 ,
    label: 'Approver-3'
  } ]
  Userthree;
  
   @UI.lineItem: [ {
    position: 70 ,
    importance: #MEDIUM,
    label: 'Alternative Approver-3'
  } ]
  @UI.identification: [ {
    position: 70 ,
    label: 'Alternative Approver-3'
  } ]
  altUserthree;

  @UI.lineItem: [ {
    position: 80 ,
    importance: #MEDIUM,
    label: 'Approver-4'
  } ]
  @UI.identification: [ {
    position: 80 ,
    label: 'Approver-4'
  } ]
  Userfour;
  
  @UI.lineItem: [ {
    position: 90 ,
    importance: #MEDIUM,
    label: 'Alternative Approver-4'
  } ]
  @UI.identification: [ {
    position: 90 ,
    label: 'Alternative Approver-4'
  } ]
  altUserfour;

  @UI.lineItem: [ {
    position: 100 ,
    importance: #MEDIUM,
    label: 'Approver-5'
  } ]
  @UI.identification: [ {
    position: 100 ,
    label: 'Approver-5'
  } ]
  Userfive;
  
   @UI.lineItem: [ {
    position: 110 ,
    importance: #MEDIUM,
    label: 'Alternative Approver-5'
  } ]
  @UI.identification: [ {
    position: 110 ,
    label: 'Alternative Approver-5'
  } ]
  altUserfive;

  @UI.hidden: true
  LocalInstanceLastChangedAt;
}
*******************************************************************************************************************
@Metadata.layer: #CORE
@UI: {
  headerInfo: {
    typeName: 'Workflow Log',
    typeNamePlural: 'Workflow Logs'
  }
}
annotate view ZC_FIWFLOG with
{
  @UI.facet: [ {
    id: 'idIdentification',
    type: #IDENTIFICATION_REFERENCE,
    label: 'Workflow Log',
    position: 10
  } ]
  @UI.lineItem: [ {
    position: 10 ,
    importance: #MEDIUM,
    label: 'WorkflowID'
  } ]
  @UI.identification: [ {
    position: 10 ,
    label: 'WorkflowID'
  } ]
  WorkflowID;

  @UI.lineItem: [ {
    position: 20 ,
    importance: #MEDIUM,
    label: ''
  } ]
  @UI.identification: [ {
    position: 20 ,
    label: ''
  } ]
     @UI.selectionField: [{ position: 10 }]
  Documentno;

  @UI.lineItem: [ {
    position: 30 ,
    importance: #MEDIUM,
    label: 'Application'
  } ]
  @UI.identification: [ {
    position: 30 ,
    label: 'Application'
  } ]
    @UI.selectionField: [{ position: 20 }]
  Application;

  @UI.lineItem: [ {
    position: 40 ,
    importance: #MEDIUM,
    label: 'Doccreated by'
  } ]
  @UI.identification: [ {
    position: 40 ,
    label: 'Doccreated by'
  } ]
     @UI.selectionField: [{ position: 30 }]
  Doccreatedby;

  @UI.lineItem: [ {
    position: 50 ,
    importance: #MEDIUM,
    label: 'Doccreated on'
  } ]
  @UI.identification: [ {
    position: 50 ,
    label: 'Doccreated on'
  } ]
     @UI.selectionField: [{ position: 40 }]
  Doccreatedon;

  @UI.lineItem: [ {
    position: 60 ,
    importance: #MEDIUM,
    label: 'Wflevel'
  } ]
  @UI.identification: [ {
    position: 60 ,
    label: 'Wflevel'
  } ]
  Wflevel;

  @UI.lineItem: [ {
    position: 70 ,
    importance: #MEDIUM,
    label: 'Status - 1'
  } ]
  @UI.identification: [ {
    position: 70 ,
    label: 'Status - 1'
  } ]
  Status;

  @UI.lineItem: [ {
    position: 80 ,
    importance: #MEDIUM,
    label: 'Approver-1'
  } ]
  @UI.identification: [ {
    position: 80 ,
    label: 'Approver-1'
  } ]
  Userone;

  @UI.lineItem: [ {
    position: 90 ,
    importance: #MEDIUM,
    label: 'Alternative Approver-1'
  } ]
  @UI.identification: [ {
    position: 90 ,
    label: 'Alternative Approver-1'
  } ]
  altuserone;

  @UI.lineItem: [ {
    position: 100 ,
    importance: #MEDIUM,
    label: 'Approvaldateu1'
  } ]
  @UI.identification: [ {
    position: 100 ,
    label: 'Approvaldateu1'
  } ]

  Approvaldateu1;

  @UI.lineItem: [ {
    position: 110 ,
    importance: #MEDIUM,
    label: 'Remarksu1'
  } ]
  @UI.identification: [ {
    position: 110 ,
    label: 'Remarksu1'
  } ]

  Remarksu1;

  @UI.lineItem: [ {
    position: 120 ,
    importance: #MEDIUM,
    label: 'Status - 2'
  } ]
  @UI.identification: [ {
    position: 120 ,
    label: 'Status - 2'
  } ]
  status2;

  @UI.lineItem: [ {
    position: 130 ,
    importance: #MEDIUM,
    label: 'Approver-2'
  } ]
  @UI.identification: [ {
    position: 130 ,
    label: 'Approver-2'
  } ]
  Usertwo;

  @UI.lineItem: [ {
     position: 140 ,
     importance: #MEDIUM,
     label: 'Alternative Approver-2'
   } ]
  @UI.identification: [ {
    position: 140 ,
    label: 'Alternative Approver-2'
  } ]
  altusertwo;

  @UI.lineItem: [ {
    position: 150 ,
      importance: #MEDIUM,
    label: 'Approvaldateu2'
  } ]
  @UI.identification: [ {
    position: 150 ,
    label: 'Approvaldateu2'
  } ]
  Approvaldateu2;

  @UI.lineItem: [ {
    position: 160 ,
    importance: #MEDIUM,
    label: 'Remarksu2'
  } ]
  @UI.identification: [ {
    position: 160 ,
    label: 'Remarksu2'
  } ]
  Remarksu2;
  @UI.lineItem: [ {
      position: 170 ,
      importance: #MEDIUM,
      label: 'Status - 3'
    } ]
  @UI.identification: [ {
    position: 170 ,
    label: 'Status - 3'
  } ]
  status3;

  @UI.lineItem: [ {
    position: 180 ,
    importance: #MEDIUM,
    label: 'Approver-3'
  } ]
  @UI.identification: [ {
    position: 180 ,
    label: 'Approver-3'
  } ]
  Userthree;

  @UI.lineItem: [ {
     position: 190 ,
     importance: #MEDIUM,
     label: 'Alternative Approver-3'
   } ]
  @UI.identification: [ {
    position: 190 ,
    label: 'Alternative Approver-3'
  } ]
  altuserthree;

  @UI.lineItem: [ {
    position: 200 ,
    importance: #MEDIUM,
    label: 'Approvaldateu3'
  } ]
  @UI.identification: [ {
    position: 200 ,
    label: 'Approvaldateu3'
  } ]


  Approvaldateu3;

  @UI.lineItem: [ {
    position: 210 ,
    importance: #MEDIUM,
    label: 'Remarksu3'
  } ]
  @UI.identification: [ {
    position: 210 ,
    label: 'Remarksu3'
  } ]
  Remarksu3;

  @UI.lineItem: [ {
    position: 220 ,
    importance: #MEDIUM,
    label: 'Approver-4'
  } ]
  @UI.identification: [ {
    position: 220 ,
    label: 'Approver-4'
  } ]
  Userfour;


  @UI.lineItem: [ {
     position: 230 ,
     importance: #MEDIUM,
     label: 'Alternative Approver-4'
   } ]
  @UI.identification: [ {
    position: 230 ,
    label: 'Alternative Approver-4'
  } ]
  altuserfour;

  @UI.lineItem: [ {
    position: 240 ,
    importance: #MEDIUM,
    label: 'Approvaldateu4'
  } ]
  @UI.identification: [ {
    position: 240 ,
    label: 'Approvaldateu4'
  } ]
  Approvaldateu4;

  @UI.lineItem: [ {
    position: 250 ,
    importance: #MEDIUM,
    label: 'Remarksu4'
  } ]
  @UI.identification: [ {
    position: 250 ,
    label: 'Remarksu4'
  } ]
  Remarksu4;

  @UI.lineItem: [ {
      position: 260 ,
      importance: #MEDIUM,
      label: 'Status - 5'
    } ]
  @UI.identification: [ {
    position: 260 ,
    label: 'Status - 5'
  } ]
  status5;

  @UI.lineItem: [ {
    position: 270 ,
    importance: #MEDIUM,
    label: 'Approver-5'
  } ]
  @UI.identification: [ {
    position: 270 ,
    label: 'Approver-5'
  } ]
  Userfive;

  @UI.lineItem: [ {
     position: 280 ,
     importance: #MEDIUM,
     label: 'Alternative Approver-5'
   } ]
  @UI.identification: [ {
    position: 280 ,
    label: 'Alternative Approver-5'
  } ]
  altuserfive;

  @UI.lineItem: [ {
    position: 290 ,
    importance: #MEDIUM,
    label: 'Approvaldateu5'
  } ]
  @UI.identification: [ {
    position: 290 ,
    label: 'Approvaldateu5'
  } ]
  Approvaldateu5;

  @UI.lineItem: [ {
    position: 300 ,
    importance: #MEDIUM,
    label: 'Remarksu5'
  } ]
  @UI.identification: [ {
    position: 300 ,
    label: 'Remarksu5'
  } ]
  Remarksu5;
  @UI.lineItem: [ {
    position: 310 ,
    importance: #MEDIUM,
    label: 'Manual Flag'
  } ]
  @UI.identification: [ {
    position: 310 ,
    label: 'Manual Flag'
  } ]
  manualflag;

  @UI.hidden: true
  LocalInstanceLastChangedAt;
}
*****************************************************************************************************************************
Database Tables 

@EndUserText.label : 'Maintain Amount limits to trigger the workflow'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table zfiamountlimit {

  key businesskey : include zfiamountlimit_key not null;
  businessdata    : include zfiamountlimit_data;
  admindata       : include zfiamountlimit_admin_data;@EndUserText.label : '##GENERATED Amount limit'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table zfiamountlimit_d {

  key mandt                  : mandt not null;
  key application            : abap.char(40) not null;
  key uuid                   : sysuuid_x16 not null;
  fromdate                   : datum;
  todate                     : datum;
  @Semantics.amount.currencyCode : 'zfiamountlimit_d.uom'
  fromamount                 : abap.curr(23,2) not null;
  @Semantics.amount.currencyCode : 'zfiamountlimit_d.uom'
  toamount                   : abap.curr(23,2) not null;
  uom                        : waers;
  @EndUserText.label : 'Level'
  wflevel                    : abap.char(2);
  lastchangedby              : abp_lastchange_user;
  lastchangedat              : abp_lastchange_tstmpl;
  localinstancelastchangedat : abp_locinst_lastchange_tstmpl;
  "%admin"                   : include sych_bdl_draft_admin_inc;

}
**************************************************************************************************************************************

@EndUserText.label : 'Approvals for FI Document Posting'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table zfidocusers {

  key businesskey : include zfidocusers_key not null;
  businessdata    : include zfidocusers_data;
  admindata       : include zfidocusers_admin_data;

}
*********************************************************************************************************************************

@EndUserText.label : '##GENERATED Maint User for Approval'
@AbapCatalog.enhancement.category : #EXTENSIBLE_ANY
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table zfidocusers_d {

  key mandt                  : mandt not null;
  @EndUserText.label : 'Application Name'
  key application            : abap.char(40) not null;
  @EndUserText.label : 'UserOne'
  userone                    : abap.char(20);
  @EndUserText.label : 'Alternative UserOne'
  altuserone                 : abap.char(20);
  @EndUserText.label : 'UserTwo'
  usertwo                    : abap.char(20);
  @EndUserText.label : 'Alternative UserTwo'
  altusertwo                 : abap.char(20);
  @EndUserText.label : 'UserThree'
  userthree                  : abap.char(20);
  @EndUserText.label : 'Alternative UserThree'
  altuserthree               : abap.char(20);
  @EndUserText.label : 'UserFour'
  userfour                   : abap.char(20);
  @EndUserText.label : 'Alternative UserFour'
  altuserfour                : abap.char(20);
  @EndUserText.label : 'UserFive'
  userfive                   : abap.char(20);
  @EndUserText.label : 'Alternative UserFive'
  altuserfive                : abap.char(20);
  createdby                  : abp_creation_user;
  createdat                  : abp_creation_tstmpl;
  lastchangedby              : abp_lastchange_user;
  lastchangedat              : abp_lastchange_tstmpl;
  localinstancelastchangedat : abp_locinst_lastchange_tstmpl;
  "%admin"                   : include sych_bdl_draft_admin_inc;

}
*************************************************************************************************************************************************

@EndUserText.label : 'FI Doc Approval log Details'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table zfiwflog {

  key businesskey : include zfiwflog_key not null;
  businessdata    : include zfiwflog_data;
  admindata       : include zfiwflog_admin_data;

}
************************************************************************************************************************************************

Strcutures

@EndUserText.label : 'Admin Fields'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
define structure zfiamountlimit_admin_data {

  last_changed_by                : abp_lastchange_user;
  last_changed_at                : abp_lastchange_tstmpl;
  local_instance_last_changed_at : abp_locinst_lastchange_tstmpl;

}
**************************************************************************************************************************************************

@EndUserText.label : 'Amount limit data Fields'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
define structure zfiamountlimit_data {

  fromdate       : datum;
  todate         : datum;
  uom            : waers;
  @Semantics.amount.currencyCode : 'zfiamountlimit_data.uom'
  key fromamount : abap.curr(23,2);
  @EndUserText.label : 'To Amount'
  @Semantics.amount.currencyCode : 'zfiamountlimit_data.uom'
  key toamount   : abap.curr(23,2);
  @EndUserText.label : 'Level'
  wflevel        : abap.char(2);

}
****************************************************************************************************************************************************

@EndUserText.label : 'Amount limit keys'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
define structure zfiamountlimit_key {

  key client      : mandt not null;
  @EndUserText.label : 'Application'
  key application : abap.char(40);
  key uuid        : sysuuid_x16 not null;

}
***************************************************************************************************************************************************************

@EndUserText.label : 'Admit Data'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
define structure zfidocusers_admin_data {

  created_by                     : abp_creation_user;
  created_at                     : abp_creation_tstmpl;
  last_changed_by                : abp_lastchange_user;
  last_changed_at                : abp_lastchange_tstmpl;
  local_instance_last_changed_at : abp_locinst_lastchange_tstmpl;

}
***********************************************************************************************************************************************************************

@EndUserText.label : 'FI Document Approval Details'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
define structure zfidocusers_data {

  @EndUserText.label : 'UserOne'
  userone      : abap.char(20);
  @EndUserText.label : 'Alternative UserOne'
  altuserone   : abap.char(20);
  @EndUserText.label : 'UserTwo'
  usertwo      : abap.char(20);
  @EndUserText.label : 'Alternative UserTwo'
  altusertwo   : abap.char(20);
  @EndUserText.label : 'UserThree'
  userthree    : abap.char(20);
  @EndUserText.label : 'Alternative UserThree'
  altuserthree : abap.char(20);
  @EndUserText.label : 'UserFour'
  userfour     : abap.char(20);
  @EndUserText.label : 'Alternative UserFour'
  altuserfour  : abap.char(20);
  @EndUserText.label : 'UserFive'
  userfive     : abap.char(20);
  @EndUserText.label : 'Alternative UserFive'
  altuserfive  : abap.char(20);

}
****************************************************************************************************************************************************

@EndUserText.label : 'Keys for FI Document Posting Approval'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
define structure zfidocusers_key {

  key client      : mandt not null;
  key application : abap.char(40) not null;

}
**************************************************************************************************************************************

@EndUserText.label : 'Admin Fields'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
define structure zfiwflog_admin_data {

  last_changed_by                : abp_lastchange_user;
  last_changed_at                : abp_lastchange_tstmpl;
  local_instance_last_changed_at : abp_locinst_lastchange_tstmpl;

}
*****************************************************************************************************************************************

@EndUserText.label : 'Data Fields Details for Workflow Log'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
define structure zfiwflog_data {

  @EndUserText.label : 'Document Created By'
  doccreatedby   : abap.char(12);
  @EndUserText.label : 'Document Created On'
  doccreatedon   : abap.dats;
  @EndUserText.label : 'Level'
  wflevel        : abap.char(2);
  @EndUserText.label : 'Approval One Status'
  status         : abap.char(10);
  @EndUserText.label : 'UserOne'
  userone        : abap.char(20);
  @EndUserText.label : 'AltUserOne'
  altuserone     : abap.char(20);
  @EndUserText.label : 'Approval Date One'
  approvaldateu1 : abap.dats;
  @EndUserText.label : 'Approval One Remarks'
  remarksu1      : abap.char(100);
  @EndUserText.label : 'Approval Two Status'
  status2        : abap.char(10);
  @EndUserText.label : 'UserTwo'
  usertwo        : abap.char(20);
  @EndUserText.label : 'AltUserTwo'
  altusertwo     : abap.char(20);
  @EndUserText.label : 'Approval Date Two'
  approvaldateu2 : abap.dats;
  @EndUserText.label : 'Approval Two Remarks'
  remarksu2      : abap.char(100);
  @EndUserText.label : 'Approval Three Status'
  status3        : abap.char(10);
  @EndUserText.label : 'UserThree'
  userthree      : abap.char(20);
  @EndUserText.label : 'AltUserThree'
  altuserthree   : abap.char(20);
  @EndUserText.label : 'Approval Date Three'
  approvaldateu3 : abap.dats;
  @EndUserText.label : 'Approval Three Remarks'
  remarksu3      : abap.char(100);
  @EndUserText.label : 'Approval Four Status'
  status4        : abap.char(10);
  @EndUserText.label : 'UserFour'
  userfour       : abap.char(20);
  @EndUserText.label : 'AltUserFour'
  altuserfour    : abap.char(20);
  @EndUserText.label : 'Approval Date Four'
  approvaldateu4 : abap.dats;
  @EndUserText.label : 'Approval Four Remarks'
  remarksu4      : abap.char(100);
  @EndUserText.label : 'Approval Five Status'
  status5        : abap.char(10);
  @EndUserText.label : 'UserFive'
  userfive       : abap.char(20);
  @EndUserText.label : 'AltUserFive'
  altuserfive    : abap.char(20);
  @EndUserText.label : 'Approval Date Five'
  approvaldateu5 : abap.dats;
  @EndUserText.label : 'Approval Five Remarks'
  remarksu5      : abap.char(100);
  manualflag     : abap_boolean;

}
*************************************************************************************************************************************

@EndUserText.label : 'FI Document Approval Log Details'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
define structure zfiwflog_key {

  key client      : mandt not null;
  @EndUserText.label : 'WorkflowID'
  key workflowid  : abap.char(14);
  key documentno  : belnr_d;
  @EndUserText.label : 'Application'
  key application : abap.char(40);

}
*****************************************************************************************************************************************

Classes

class ZBP_R_FIAMOUNTLIMIT definition
  public
  abstract
  final
  for behavior of ZI_FIAMOUNTLIMIT .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZBP_R_FIAMOUNTLIMIT IMPLEMENTATION.
ENDCLASS.

}
*********************************************************************************************************************************

managed implementation in class ZBP_R_FIAMOUNTLIMIT unique;
strict ( 2 );
with draft;

define behavior for ZI_FIAMOUNTLIMIT alias AMOUNTLIMIT
persistent table zfiamountlimit
draft table zfiamountlimit_d
etag master LocalInstanceLastChangedAt
lock master total etag LastChangedAt
authorization master ( global )

{
  field ( numbering : managed )
  uuid;

  field ( mandatory : create )
  Application,
//  Fromamount,
  Fromdate,
  Todate;
//  Toamount;


  field ( readonly )
  uuid,
  LastChangedAt,
  LastChangedBy,
  LocalInstanceLastChangedAt;

  field ( readonly : update )
  Application;


 validation MandateVales on save { create; update; field Application, Fromdate,todate,
  fromamount,toamount; }

  create;
  update;
  delete;

  draft action Edit;
  draft action Activate optimized;
  draft action Discard;
  draft action Resume;
  draft determine action Prepare;

  mapping for zfiamountlimit
    {
      uuid                       = uuid;
      Application                = application;
      Fromdate                   = fromdate;
      Todate                     = todate;
      Uom                        = uom;
      Fromamount                 = fromamount;
      Toamount                   = toamount;
      wflevel                    = wflevel;
      LastChangedBy              = last_changed_by;
      LastChangedAt              = last_changed_at;
      LocalInstanceLastChangedAt = local_instance_last_changed_at;
    }
}
************************************************************************************************************************************
class ZBP_R_FIDOCUSERS definition
  public
  abstract
  final
  for behavior of ZI_FIDOCUSERS .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZBP_R_FIDOCUSERS IMPLEMENTATION.
ENDCLASS.
*************************************************************************************************

managed implementation in class ZBP_R_FIDOCUSERS unique;
strict ( 2 );
with draft;

define behavior for ZI_FIDOCUSERS alias Fidocmaint
persistent table zfidocusers
draft table zfidocusers_d
etag master LocalInstanceLastChangedAt
lock master total etag LastChangedAt
authorization master ( global )

{
  field ( mandatory : create )
  application;

  // Documenttype;

  field ( readonly )
  CreatedAt,
  CreatedBy,
  LastChangedAt,
  LastChangedBy,
  LocalInstanceLastChangedAt;

  field ( readonly : update )
  //Documenttype;

  application;
  create;
  update;
  delete;

  draft action Edit;
  draft action Activate optimized;
  draft action Discard;
  draft action Resume;
  draft determine action Prepare;

  mapping for zfidocusers
    {

      //Documenttype = documenttype;
      application                = application;
      Userone                    = userone;
      altuserone                 = altuserone;
      Usertwo                    = usertwo;
      altUsertwo                 = altusertwo;
      Userthree                  = userthree;
      altUserthree               = altuserthree;
      Userfour                   = userfour;
      altUserfour                = altuserfour;
      Userfive                   = userfive;
      altUserfive                = altuserfive;
      CreatedBy                  = created_by;
      CreatedAt                  = created_at;
      LastChangedBy              = last_changed_by;
      LastChangedAt              = last_changed_at;
      LocalInstanceLastChangedAt = local_instance_last_changed_at;
    }
}
**************************************************************************************************************************************************************************

ZFI_051_ACCDOCAPR_T3 Package 

Data Definitions 

@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Value Help for Application Names'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}

//@ObjectModel.dataCategory: #VALUE_HELP
//@ObjectModel.representativeKey: 'tcode'
//@ObjectModel.supportedCapabilities: [#SQL_DATA_SOURCE,
//                                     #CDS_MODELING_DATA_SOURCE,
//                                     #CDS_MODELING_ASSOCIATION_TARGET,
//                                     #VALUE_HELP_PROVIDER,
//                                     #SEARCHABLE_ENTITY]
//@ObjectModel.modelingPattern:#NONE
//@Search.searchable: true
//@Consumption.ranked: true
define view entity ZI_ApplicationVH
  as select from tstc as _TCODE
{
  @ObjectModel.text.element: [ 'Description' ]
  @Search.defaultSearchElement: true
  @Search.fuzzinessThreshold: 0.8
  @Search.ranking: #HIGH
 key _TCODE.tcode,
  @Semantics.text: true
  @Search.defaultSearchElement: true
  @Search.fuzzinessThreshold: 0.8
  @Search.ranking: #LOW
  case  _TCODE.tcode
  when 'FB60'
  then 'Create Incoming Invoices'
   when 'FB65'
  then 'Manage Supplier Invoices'
   when 'FB70'
  then 'Create Outgoing Invoices'
   when 'FB75'
  then 'Manage Credit Memo Requests' end as Description
}
where
     _TCODE.tcode = 'FB60'
  or _TCODE.tcode = 'FB65'
  or _TCODE.tcode = 'FB70'
  or _TCODE.tcode = 'FB75'
***************************************************************************************************************************************************

Structures 

@EndUserText.label : 'FI Approval User Details'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
define structure zsfiapproval {

  approvals : ztfidocappr;

}
******************************************************************************************************************************************************

@EndUserText.label : 'FI Document Approval Structure'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
define structure zsfidocappr {

  user            : char20;
  mail            : ad_smtpadr;
  level           : char2;
  flag            : char1;
  alternativeuser : char20;

}
************************************************************************************************************************************************

@EndUserText.label : 'Final FI Approval User Details'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
define structure zsfinalapp {

  final : ztfiapproval;

}
*************************************************************************************************************************************************

Classes

class ZFIDOCUMENTAPPROVAL definition
  public
  final
  create public .

public section.

  interfaces BI_OBJECT .
  interfaces BI_PERSISTENT .
  interfaces IF_WORKFLOW .

  types:
    BEGIN OF pendingdetail,
        workflowid   TYPE zfiwflog-workflowid,
        documentno   TYPE zfiwflog-documentno,
        application  TYPE zfiwflog-application,
        doccreatedby TYPE zfiwflog-doccreatedby,
        doccreatedon TYPE zfiwflog-doccreatedon,
        wflevel      TYPE zfiwflog-wflevel,
        status       TYPE zfiwflog-status,
        userone      TYPE zfiwflog-userone,
      END OF pendingdetail .
  types:
    tt_header TYPE TABLE OF vbkpf .

  constants ONELACK type FINS_VHCUR12 value '100000.00' ##NO_TEXT.
  constants TWOLACK type FINS_VHCUR12 value '200000.00' ##NO_TEXT.
  constants THREELACK type FINS_VHCUR12 value '300000.00' ##NO_TEXT.

  class-methods GETUSER
    importing
      value(DOCUMENTNO) type BELNR_D optional
      value(APPLICATION) type CHAR40 optional
      value(BASEAMOUNT) type DMBTR optional
    exporting
      !USERS type ZTFIDOCAPPR
      !INITIATOR type AD_NAMTEXT
      !TASKLINKS type BCSY_TEXT
      !FLAG type BOOLEAN
      !ET_USER type ZTFIDOCAPPR
      !MAXLEVEL type CHAR2 .
  class-methods APPRVED
    importing
      value(APPLICATION) type CHAR40 optional
      value(DOCUMENTNO) type BELNR_D optional
      value(BASEAMOUNT) type DMBTR optional
      !LEVEL type CHAR2 optional
    exporting
      !APPROVER type SWW_AAGENT
      !INITIATOR type AD_NAMTEXT .
  class-methods REJECTED
    importing
      value(APPLICATION) type CHAR40 optional
      value(DOCUMENTNO) type BELNR_D optional
    exporting
      !REJECTOR type SWW_AAGENT
      !INITIATOR type AD_NAMTEXT
      !EXIT type BOOLEAN .
  class-methods PENDING
    importing
      value(PENDINGSTATUSUDP) type PENDINGDETAIL optional .
  class-methods DOCUMENTPOST
    importing
      !HEADER type TT_HEADER .
  methods CALLWF
    importing
      value(DOCUMENTNO) type BELNR_D optional
      value(APPLICATION) type CHAR40 optional
      value(BASEAMOUNT) type DMBTR optional
      value(POSTINGDATE) type BUDAT optional .
  class-methods DECISIONCONTROL
    importing
      !APPLICATION type CHAR40 optional
      !DOCUMENTNO type BELNR_D optional
      !BASEAMOUNT type DMBTR optional
      !CURRENTLEVEL type CHAR2 optional
      !MAXLEVEL type CHAR2 optional
    exporting
      !FLAG type CHAR1 .
protected section.
private section.
ENDCLASS.



CLASS ZFIDOCUMENTAPPROVAL IMPLEMENTATION.


  METHOD apprved.
*----------------------------------------------------------------------*
* Class           ZFIDOCUMENTAPPROVAL
*----------------------------------------------------------------------*
* Title:          Fi Document Approval(Park to post)                   *
* RICEF#:         RSB_FI_R051                                          *
* Transaction:    FB60 , FB65 , FB70 , FB75                            *
*----------------------------------------------------------------------*
* Copyright:      NDBS, Inc.                                           *
* Client:         RSB                                                  *
*----------------------------------------------------------------------*
* Developer:      Saakshi.D                                            *
* Creation Date:  16/07/2025                                           *
* Description:    Fi Document Approval(Park to post)                   *
*----------------------------------------------------------------------*
* Modification History                                                 *
*----------------------------------------------------------------------*
* Modified by:                                                         *
* Date:                                                                *
* Transport:                                                           *
* Description:                                                         *
*----------------------------------------------------------------------*
    "  Data Declaration
    TYPES:ty_t_email_ids TYPE TABLE OF ad_smtpadr .
    DATA:wflogdetails            TYPE TABLE OF zfiwflog,
         workitem                TYPE swotobjid,
         workitemid              TYPE swotobjid-objkey,
         simplecontainers        TYPE TABLE OF swr_cont,
         messagelines            TYPE TABLE OF swr_messag,
         messagestructs          TYPE TABLE OF swr_mstruc,
         lsubcontainerborobjects TYPE TABLE OF swr_cont,
         subcontainerallobjects  TYPE TABLE OF swr_cont,
         wkitemid                TYPE swr_struct-workitemid,
         documentid              TYPE sofolenti1-doc_id,
         objectcontents          TYPE TABLE OF solisti1,
         header                  TYPE TABLE OF vbkpf,
         maxlevel                TYPE zfiwflog-wflevel,
         emailcontent            TYPE bcsy_text,
         emailids_to             TYPE yglobalutility=>tt_email_ids.
    CONSTANTS: linebreak TYPE c LENGTH 8 VALUE '<br><br>'.

    IF application = 'Create Inc'.
      application = 'Create Incoming Invoices'.
    ELSEIF application = 'Create Out'.
      application = 'Create Outgoing Invoices'.
    ELSEIF application = 'Manage Cre'.
      application = 'Manage Credit Memo Requests'.
    ELSEIF application = 'Manage Sup'.
      application = 'Manage Supplier Invoices'.
    ENDIF.


    "  Get Workitem Details
    CALL FUNCTION 'SWE_WI_GET_FROM_REQUESTER'
      IMPORTING
        requester_workitem   = workitem
        requester_workitemid = workitemid.
    wkitemid             = workitemid.

    "  Get Container Details
    CALL FUNCTION 'SAP_WAPI_READ_CONTAINER'
      EXPORTING
        workitem_id              = wkitemid
        language                 = sy-langu
        user                     = sy-uname
      TABLES
        simple_container         = simplecontainers
        message_lines            = messagelines
        message_struct           = messagestructs
        subcontainer_bor_objects = lsubcontainerborobjects
        subcontainer_all_objects = subcontainerallobjects.

    "  Read the _ATTACH_OBJECTS element
    IF ( line_exists( subcontainerallobjects[ element = '_ATTACH_OBJECTS' ] ) ). "#EC CI_STDSEQ "_ATTACH_OBJECTS
      documentid = subcontainerallobjects[ element = '_ATTACH_OBJECTS' ]-value.
    ENDIF.
    "  Read the SOFM Document
    CALL FUNCTION 'SO_DOCUMENT_READ_API1'
      EXPORTING
        document_id                = documentid
      TABLES
        object_content             = objectcontents
      EXCEPTIONS
        document_id_not_exist      = 1
        operation_no_authorization = 2
        x_error                    = 3
        OTHERS                     = 4.
    IF sy-subrc = 0.
      "  Get Remarks
      DATA(remark) = REDUCE #( INIT remarks TYPE string
                                FOR <remarks> IN objectcontents
                                NEXT remarks = COND #( WHEN remarks IS INITIAL
                                                         THEN <remarks>-line
                                                         ELSE remarks && ',' && <remarks>-line ) ).
    ENDIF.

    "  Get Accounting Document Details
    SELECT SINGLE accountingdocument,
                  accountingdoccreatedbyuser,
                  companycode,
                  fiscalyear,
                  postingdate
                    FROM i_journalentry
                                             INTO @DATA(accdocdetail)
                                             WHERE accountingdocument = @documentno.

    IF sy-subrc  = 0.
      initiator = accdocdetail-accountingdoccreatedbyuser.
*      "  Fetch privious workitem id
      SELECT SINGLE top_wi_id            "#EC CI_NOORDER or "#EC WARNOK
                   FROM swwwihead "wi_cruser
                          INTO @DATA(priviousid)
                          WHERE wi_id = @workitemid.
      IF sy-subrc  = 0.

        "  Get user for current workitem id for without levels "auto
        SELECT SINGLE wi_aagent FROM swwwihead "#EC CI_NOORDER or "#EC WARNOK
                         INTO @approver
                         WHERE top_wi_id = @priviousid
                            AND wi_rh_task = 'TS99900004'.

        IF sy-subrc <> 0.
          "  Get user for current workitem id for without levels "manual
          SELECT SINGLE wi_aagent FROM swwwihead "#EC CI_NOORDER or "#EC WARNOK
                           INTO @approver
                           WHERE top_wi_id = @priviousid
                              AND wi_rh_task = 'TS99900009'.
        ENDIF.
*        DATA(workitemno) = workitemid - 4.
*        SELECT SINGLE top_wi_id "wi_cruser
*                    FROM swwwihead "wi_cruser
*             INTO  @DATA(topid)
*                            WHERE wi_id = @workitemno.
*        IF sy-subrc  = 0.
*          SELECT SINGLE wi_creator "wi_cruser
*               FROM swwwihead "wi_cruser
*               INTO  @initiator
*                              WHERE wi_id = @topid.
*
*        ENDIF.
        IF approver IS NOT INITIAL.
          DATA(checkapprover) = approver.
          IF approver <> 'SAP_WFRT'.
            DATA(manualappr) = abap_true.
*          ELSE.
*            DATA(autoapproveflag) = abap_true.
          ENDIF.
          SELECT SINGLE * FROM zfiwflog INTO @DATA(approvallogdetail)
                                                WHERE documentno  = @documentno
                                                  AND application = @application.

          IF sy-subrc  = 0.
            approver = COND #( WHEN approvallogdetail-status5 = TEXT-006"'PENDING'.
                               THEN approvallogdetail-userfive
                               WHEN approvallogdetail-status4 = TEXT-006"'PENDING'.
                               THEN approvallogdetail-userfour
                               WHEN approvallogdetail-status3 = TEXT-006"'PENDING'.
                               THEN approvallogdetail-userthree
                               WHEN approvallogdetail-status2 = TEXT-006"'PENDING'.
                               THEN approvallogdetail-usertwo
                               WHEN approvallogdetail-status = TEXT-006"'PENDING'.
                               THEN approvallogdetail-userone ).

            DATA(altapprover) = COND #( WHEN approvallogdetail-status5 = TEXT-006"'PENDING'.
                                THEN approvallogdetail-altuserfive
                                WHEN approvallogdetail-status4 = TEXT-006"'PENDING'.
                                THEN approvallogdetail-altuserfour
                                WHEN approvallogdetail-status3 = TEXT-006"'PENDING'.
                                THEN approvallogdetail-altuserthree
                                WHEN approvallogdetail-status2 = TEXT-006"'PENDING'.
                                THEN approvallogdetail-altusertwo
                                WHEN approvallogdetail-status = TEXT-006"'PENDING'.
                                THEN approvallogdetail-altuserone ).

          ENDIF.
          IF manualappr = abap_true.
            "Update Manual Approval Flag
            IF approvallogdetail-manualflag IS INITIAL.
              approvallogdetail-manualflag = abap_true.
              APPEND approvallogdetail TO wflogdetails.
              MODIFY zfiwflog FROM TABLE wflogdetails.
            ENDIF.
          ENDIF.


          SELECT SINGLE application,     "#EC CI_NOORDER or "#EC WARNOK
                        userone,
                        usertwo,
                        userthree,
                        userfour,
                        userfive,
                        altuserone,
                        altusertwo,
                        altuserthree,
                        altuserfour,
                        altuserfive FROM zfidocusers INTO @DATA(approvers)
                                WHERE application LIKE @application
                                  AND ( userone = @approver
                                   OR usertwo   = @approver
                                   OR userthree = @approver
                                   OR userfour  = @approver
                                   OR userfive  = @approver )
                                  AND ( altuserone = @altapprover
                                   OR altusertwo   = @altapprover
                                   OR altuserthree = @altapprover
                                   OR altuserfour  = @altapprover
                                   OR altuserfive  = @altapprover ).
          IF sy-subrc  = 0.
            SELECT SUM( dmbtr ) AS amount FROM vbsegs INTO @DATA(amount) WHERE belnr = @documentno.
            IF sy-subrc  = 0.
              SELECT SINGLE application,
                                  fromdate ,
                                  todate,
                                  fromamount,
                                  toamount,
                                  wflevel FROM zfiamountlimit
                                          INTO @DATA(amountlimit)
                                          WHERE fromdate LE @accdocdetail-postingdate AND todate GE @accdocdetail-postingdate
                                            AND fromamount LE @amount AND toamount GE @amount
                                            AND application = @application.

              IF sy-subrc = 0.
                IF amountlimit-wflevel = 'L1' OR amountlimit-wflevel = 'l1' OR amountlimit-wflevel = '1'.
                  CLEAR: approvers-userfive,approvers-userfour,approvers-userthree,approvers-usertwo,
                         approvers-altuserfive,approvers-altuserfour,approvers-altuserthree,approvers-altusertwo.

                ELSEIF amountlimit-wflevel = 'L2' OR amountlimit-wflevel = 'l2' OR amountlimit-wflevel = '2'.
                  CLEAR: approvers-userfive,approvers-userfour,approvers-userthree,
                         approvers-altuserfive,approvers-altuserfour,approvers-altuserthree.

                ELSEIF  amountlimit-wflevel = 'L3' OR amountlimit-wflevel = 'l3' OR amountlimit-wflevel = '3'.
                  CLEAR: approvers-userfive,approvers-userfour,approvers-altuserfive,approvers-altuserfour.

                ELSEIF amountlimit-wflevel = 'L4' OR amountlimit-wflevel = 'l4' OR amountlimit-wflevel = '4'.
                  CLEAR: approvers-userfive,approvers-altuserfive.
                ENDIF.
              ENDIF.

              maxlevel = amountlimit-wflevel.
            ENDIF.
          ENDIF.

*          SELECT SINGLE * FROM zfiwflog INTO @DATA(approvallogdetail)
*                                        WHERE documentno  = @documentno
*                                          AND application = @application.



          IF approvallogdetail IS NOT INITIAL AND approvallogdetail-wflevel = 'L1'.

            approvallogdetail-status = TEXT-004.     "Approved
            approvallogdetail-approvaldateu1 = sy-datum .
            approvallogdetail-remarksu1 = remark .

            APPEND approvallogdetail TO wflogdetails.

          ELSEIF approvallogdetail-wflevel = 'L2'.

            approvallogdetail-status2 = TEXT-004.     "Approved
            approvallogdetail-approvaldateu2 = sy-datum .
            approvallogdetail-remarksu2 = remark .

            APPEND approvallogdetail TO wflogdetails.

          ELSEIF approvallogdetail-wflevel = 'L3'.
            approvallogdetail-status3 = TEXT-004.     "Approved
            approvallogdetail-approvaldateu3 = sy-datum .
            approvallogdetail-remarksu3 = remark .

            APPEND approvallogdetail TO wflogdetails.

          ELSEIF approvallogdetail-wflevel = 'L4'.
            approvallogdetail-status4 = TEXT-004.     "Approved
            approvallogdetail-approvaldateu4 = sy-datum .
            approvallogdetail-remarksu4 = remark .

            APPEND approvallogdetail TO wflogdetails.

          ELSEIF approvallogdetail-wflevel = 'L5' .

            approvallogdetail-status5 = TEXT-004.     "Approved
            approvallogdetail-approvaldateu5 = sy-datum .
            approvallogdetail-remarksu5 = remark .

            APPEND approvallogdetail TO wflogdetails.

          ENDIF.
        ENDIF.
      ENDIF.
    ENDIF.


    IF wflogdetails IS NOT INITIAL.
      IF maxlevel = VALUE #( wflogdetails[ 1 ]-wflevel OPTIONAL ).

        IF approvallogdetail-manualflag IS NOT INITIAL AND ( checkapprover <> approver AND checkapprover <> altapprover ).

          SELECT a~smtp_addr FROM adr6 AS a INNER JOIN usr21 ON usr21~addrnumber = a~addrnumber
                                                         AND usr21~persnumber = a~persnumber
                                                    INTO TABLE @emailids_to WHERE usr21~bname IN ( @altapprover, @approver ).
          IF sy-subrc  = 0.
          ELSE.
          ENDIF.

          emailcontent =   VALUE #( ( line = | { TEXT-017 }| )
                                    ( line = linebreak )
                                    ( line = |{ TEXT-021 } { | | }{ documentno } { | | } { TEXT-024 } | )
                                    ( line = linebreak )
*                                    ( line = TEXT-022 )
*                                    ( line = linebreak )
                                    ( line = TEXT-023 ) ).

          "Trigger Email
          TRY.
              yglobalutility=>sendemail(
                EXPORTING
                  sender      = 'SAP.NOTIFICATION@RSBGLOBAL.COM'              " E-Mail Address
                  subject     = 'Attention: AUTO APPROVAL INTIMATION.'        " Maintenance Notification Alert for Notification number
                  emailids_to = emailids_to                                   " TO
                  email_body  = emailcontent                                  " Email Body  " CC
                IMPORTING
                  sucess_flag = DATA(flag)
                  message     = DATA(message) ).
            CATCH cx_root.
          ENDTRY.
        ENDIF.
        " Posting parked document
        APPEND VALUE #( belnr = accdocdetail-accountingdocument
                        bukrs = accdocdetail-companycode
                        gjahr = accdocdetail-fiscalyear
                        budat = sy-datum ) TO header.
        TRY.
            zfidocumentapproval=>documentpost( header = header ).
          CATCH cx_root.
        ENDTRY.
      ENDIF.

      TRY.
          MODIFY zfiwflog FROM TABLE wflogdetails.
          IF sy-subrc  = 0.
            COMMIT WORK.
          ENDIF.
        CATCH cx_root.
      ENDTRY.
    ENDIF.
























*****          IF approvallogdetail IS NOT INITIAL AND approvallogdetail-wflevel = 'L1' AND approvallogdetail-status <> TEXT-006.
*****
*****            wflogdetails =  VALUE #( ( workflowid     = approvallogdetail-workflowid
*****                                       documentno     = approvallogdetail-documentno
*****                                       application    = approvallogdetail-application
*****                                       doccreatedby   = approvallogdetail-doccreatedby
*****                                       doccreatedon   = approvallogdetail-doccreatedon
*****                                       wflevel        = 'L2'
*****                                       status         = TEXT-004     "Approved
*****                                       userone        = approvallogdetail-userone
*****                                       altuserone     = approvallogdetail-altuserone
*****                                       approvaldateu1 = approvallogdetail-approvaldateu1
*****                                       remarksu1      = approvallogdetail-remarksu1
*****                                       usertwo        = approver
*****                                       altusertwo     = altapprover
*****                                       approvaldateu2 = sy-datum
*****                                       remarksu2      = remark ) ).
*****
*****          ELSEIF approvallogdetail-wflevel = 'L2'.
*****            wflogdetails =  VALUE #( ( workflowid     = approvallogdetail-workflowid
*****                                       documentno     = approvallogdetail-documentno
*****                                       application    = approvallogdetail-application
*****                                       doccreatedby   = approvallogdetail-doccreatedby
*****                                       doccreatedon   = approvallogdetail-doccreatedon
*****                                       wflevel        = 'L3'
******                                       status         = TEXT-004      "Approved
*****                                       status         = approvallogdetail-status
*****                                       userone        = approvallogdetail-userone
*****                                       altuserone     = approvallogdetail-altuserone
*****                                       approvaldateu1 = approvallogdetail-approvaldateu1
*****                                       remarksu1      = approvallogdetail-remarksu1
*****                                       status2        = TEXT-004       "Approved
*****                                       usertwo        = approvallogdetail-usertwo
*****                                       altusertwo     = approvallogdetail-altusertwo
*****                                       approvaldateu2 = approvallogdetail-approvaldateu2
*****                                       remarksu2      = approvallogdetail-remarksu2
*****                                       userthree      = approver
*****                                       altuserthree   = altapprover
*****                                       approvaldateu3 = sy-datum
*****                                       remarksu3      = remark ) ).
*****
*****          ELSEIF approvallogdetail-wflevel = 'L3'.
*****            wflogdetails =  VALUE #( ( workflowid     = approvallogdetail-workflowid
*****                                       documentno     = approvallogdetail-documentno
*****                                       application    = approvallogdetail-application
*****                                       doccreatedby   = approvallogdetail-doccreatedby
*****                                       doccreatedon   = approvallogdetail-doccreatedon
*****                                       wflevel        = 'L4'
******                                       status         = TEXT-004      "Approved
*****                                       status         = approvallogdetail-status     "Approved
*****                                       userone        = approvallogdetail-userone
*****                                       altuserone     = approvallogdetail-altuserone
*****                                       approvaldateu1 = approvallogdetail-approvaldateu1
*****                                       remarksu1      = approvallogdetail-remarksu1
*****                                       status2        = approvallogdetail-status2
*****                                       usertwo        = approvallogdetail-usertwo
*****                                       altusertwo     = approvallogdetail-altusertwo
*****                                       approvaldateu2 = approvallogdetail-approvaldateu2
*****                                       remarksu2      = approvallogdetail-remarksu2
*****                                       userthree      = approvallogdetail-userthree
*****                                       altuserthree   = approvallogdetail-altuserthree
*****                                       approvaldateu3 = approvallogdetail-approvaldateu3
*****                                       remarksu3      = approvallogdetail-remarksu3
*****                                       status3        = TEXT-004        "Approved
*****                                       userfour       = approver
*****                                       altuserfour    = altapprover
*****                                       approvaldateu4 = sy-datum
*****                                       remarksu4      = remark ) ).
*****
*****          ELSEIF approvallogdetail-wflevel = 'L4'.
*****            wflogdetails =  VALUE #( ( workflowid     = approvallogdetail-workflowid
*****                                       documentno     = approvallogdetail-documentno
*****                                       application    = approvallogdetail-application
*****                                       doccreatedby   = approvallogdetail-doccreatedby
*****                                       doccreatedon   = approvallogdetail-doccreatedon
*****                                       wflevel        = 'L5'
******                                       status         = TEXT-004      "Approved
*****                                       status         = approvallogdetail-status
*****                                       userone        = approvallogdetail-userone
*****                                       altuserone     = approvallogdetail-altuserone
*****                                       approvaldateu1 = approvallogdetail-approvaldateu1
*****                                       remarksu1      = approvallogdetail-remarksu1
*****                                       usertwo        = approvallogdetail-usertwo
*****                                       altusertwo     = approvallogdetail-altusertwo
*****                                       approvaldateu2 = approvallogdetail-approvaldateu2
*****                                       remarksu2      = approvallogdetail-remarksu2
*****                                       status2        = approvallogdetail-status2
*****                                       userthree      = approvallogdetail-userthree
*****                                       approvaldateu3 = approvallogdetail-approvaldateu3
*****                                       remarksu3      = approvallogdetail-remarksu3
*****                                       status3        = approvallogdetail-status3
*****                                       userfour       = approvallogdetail-userfour
*****                                       approvaldateu4 = approvallogdetail-approvaldateu4
*****                                       remarksu4      = approvallogdetail-remarksu4
*****                                       status4        = approvallogdetail-status4
*****                                       userfive       = approver
*****                                       altuserfive    = altapprover
*****                                       approvaldateu5 = sy-datum
*****                                       remarksu5      = remark
*****                                       status5        = TEXT-004 ) ).      "Approved ) ).
*****
*****          ELSEIF approvallogdetail-wflevel = 'L1' AND approvallogdetail-status = TEXT-006. "PENDING
*****
*****            wflogdetails =   VALUE #( ( workflowid     = TEXT-007 "WORKFLOWID
*****                                        documentno     = approvallogdetail-documentno
*****                                        application    = approvallogdetail-application
*****                                        doccreatedby   = approvallogdetail-doccreatedby
*****                                        doccreatedon   = approvallogdetail-doccreatedon
*****                                        wflevel        = 'L1'
*****                                        status         = TEXT-004    "Approved
*****                                        userone        = approver
*****                                        altuserone     = altapprover
*****                                        approvaldateu1 = sy-datum
*****                                        remarksu1      = remark ) ).
*****
*****          ENDIF.
*****        ENDIF.
*****      ENDIF.
*****    ENDIF.
*****
*****
*****
*****    IF wflogdetails IS NOT INITIAL.
*****      IF maxlevel = VALUE #( wflogdetails[ 1 ]-wflevel OPTIONAL ).
*****
*****        " Posting parked document
*****        APPEND VALUE #( belnr = accdocdetail-accountingdocument
*****                        bukrs = accdocdetail-companycode
******                                               tcode =    rec_belnr-tcode.
*****                        gjahr = accdocdetail-fiscalyear
*****                        budat = sy-datum ) TO header.
*****        TRY.
*****            zfidocumentapproval=>documentpost( header = header ).
*****          CATCH cx_root.
*****        ENDTRY.
*****      ENDIF.
*****
*****      TRY.
*****          MODIFY zfiwflog FROM TABLE wflogdetails.
*****          IF sy-subrc  = 0.
*****            COMMIT WORK.
*****          ENDIF.
*****        CATCH cx_root.
*****      ENDTRY.
*****    ENDIF.
  ENDMETHOD.


  METHOD callwf.
    "Data Declaration
    DATA: message_lines   TYPE TABLE OF swr_messag,
          message_struct  TYPE TABLE OF swr_mstruc,
          agents          TYPE TABLE OF swragent,
          return_code     TYPE sy-subrc,
          workitem_id     TYPE swr_struct-workitemid,
          new_status      TYPE swr_wistat,
          input_container TYPE TABLE OF swr_cont,
          event           TYPE swr_struct-event,
          object_type     TYPE swr_struct-object_typ,
          basevalue       TYPE string.

    basevalue =  baseamount .
    input_container = VALUE #( ( element = TEXT-009 "'DOCUMENTNO'
                                 value   = documentno )
                               ( element = TEXT-008 "'APPLICATION'
                                 value   = application )
                               ( element = TEXT-015
                                 value   = basevalue ) ).


    SELECT SINGLE application,
                    fromdate ,
                    todate,
                    fromamount,
                    toamount  FROM zfiamountlimit
        INTO @DATA(amountlimit)
        WHERE fromdate LE  @postingdate AND todate GE @postingdate
          AND   fromamount LE @baseamount AND toamount GE @baseamount
        AND application = @application.

    IF sy-subrc = 0.
    ELSE.
    ENDIF.
*    IF baseamount >= zfidocumentapproval=>onelack.
    IF baseamount BETWEEN amountlimit-fromamount AND amountlimit-toamount.

      CALL FUNCTION 'SAP_WAPI_START_WORKFLOW'
        EXPORTING
          task            = 'WS99900002'
          language        = sy-langu
          user            = sy-uname
        IMPORTING
          return_code     = return_code
          workitem_id     = workitem_id
          new_status      = new_status
        TABLES
          input_container = input_container
          message_lines   = message_lines
          message_struct  = message_struct
          agents          = agents.
    ENDIF.
  ENDMETHOD.


  METHOD decisioncontrol.

    IF maxlevel = currentlevel.
      SELECT SINGLE * FROM zfiwflog INTO @DATA(approvallogdetail)
                                                      WHERE documentno  = @documentno
                                                        AND application = @application.

      IF sy-subrc  = 0 AND  approvallogdetail-manualflag = abap_true.
        flag = abap_true.
      ENDIF.
    ELSE.
      flag = abap_true.
    ENDIF.
  ENDMETHOD.


  METHOD documentpost.

    DATA:msges  TYPE TABLE of fimsg1,
         msgtab TYPE TABLE of msg_tab_line.


    CALL FUNCTION 'PRELIMINARY_POSTING_POST_ALL'
      EXPORTING
        nomsg     = abap_true
      TABLES
        t_vbkpf   = header
        t_msg     = msges
        t_msg_tab = msgtab.

    IF line_exists( msgtab[ posted = /isdfps/cl_const_abc_123=>gc_x ] ) . "#EC CI_STDSEQ

      CALL FUNCTION 'BAPI_TRANSACTION_COMMIT'
        EXPORTING
          wait = abap_true.
    ENDIF.
  ENDMETHOD.


  METHOD getuser.
*----------------------------------------------------------------------*
* Class           ZFIDOCUMENTAPPROVAL
*----------------------------------------------------------------------*
* Title:          Fi Document Approval(Park to post)                   *
* RICEF#:         RSB_FI_R051                                          *
* Transaction:    FB60 , FB65 , FB70 , FB75                            *
*----------------------------------------------------------------------*
* Copyright:      NDBS, Inc.                                           *
* Client:         RSB                                                  *
*----------------------------------------------------------------------*
* Developer:      Saakshi.D                                            *
* Creation Date:  16/07/2025                                           *
* Description:    Fi Document Approval(Park to post)                   *
*----------------------------------------------------------------------*
* Modification History                                                 *
*----------------------------------------------------------------------*
* Modified by:                                                         *
* Date:                                                                *
* Transport:                                                           *
* Description:                                                         *
*----------------------------------------------------------------------*
    "  Data Declaration
    DATA:tasklink     TYPE soli,
         approvalstab TYPE ztfiapproval,
         approval     TYPE ztfidocappr,
         sencond      TYPE zsfiapproval,
         finals       TYPE zsfinalapp.

    CHECK documentno IS NOT INITIAL.
    "Maultilevel flag
    flag = abap_true.
    "  Get Parked Document Details Details
    SELECT SINGLE accountingdocument, "belnr
                  accountingdoccreatedbyuser,  "usnam
                  postingdate "budat
                  FROM i_journalentry AS bkpf INTO @DATA(accdocdetail) WHERE accountingdocument = @documentno.
    IF sy-subrc EQ 0.

      "  Get address number from  USR21
      SELECT SINGLE addressid,
                    addresspersonid FROM i_user
                               INTO @DATA(user_add)
                               WHERE userid = @accdocdetail-accountingdoccreatedbyuser.

      IF sy-subrc = 0.

        "  Get full name from ADRP using the address number and person number
        SELECT SINGLE  name_text FROM adrp
                                WHERE persnumber = @user_add-addresspersonid
                                INTO @initiator.
      ENDIF.

*      "Get Base amount
*      SELECT SUM( amountincompanycodecurrency ) FROM i_glaccountlineitemrawdata AS acdoca
*                        WHERE debitcreditcode = @/isdfps/cl_const_abc_123=>gc_s
*                        INTO @DATA(baseamount).
*      IF sy-subrc = 0.
*      ELSE.
*      ENDIF.

      "  Get Users / Approvers
      SELECT SINGLE application,
                    userone,
                    usertwo,
                    userthree,
                    userfour,
                    userfive FROM zfidocusers INTO @DATA(approverdetail)
                                  WHERE application = @application.
      IF sy-subrc  = 0.
        "Restrict users based on the base amount


        SELECT SINGLE application,
                     fromdate ,
                     todate,
                     fromamount,
                     toamount,
                     wflevel
                       FROM zfiamountlimit
         INTO @DATA(amountlimit)
         WHERE fromdate LE @accdocdetail-postingdate AND todate GE @accdocdetail-postingdate
          AND   fromamount LE @baseamount AND toamount GE @baseamount
*         WHERE fromdate GE  @accdocdetail-postingdate AND todate LE @accdocdetail-postingdate
         AND application = @application.

        IF sy-subrc = 0.
          maxlevel = amountlimit-wflevel.
          IF amountlimit-wflevel = 'L1' OR amountlimit-wflevel = 'l1' OR amountlimit-wflevel = '1'.
            CLEAR: approverdetail-userfive,approverdetail-userfour,approverdetail-userthree,approverdetail-usertwo.
          ELSEIF amountlimit-wflevel = 'L2' OR amountlimit-wflevel = 'l2' OR amountlimit-wflevel = '2'.
            CLEAR: approverdetail-userfive,approverdetail-userfour,approverdetail-userthree.
          ELSEIF  amountlimit-wflevel = 'L3' OR amountlimit-wflevel = 'l3' OR amountlimit-wflevel = '3'.
            CLEAR: approverdetail-userfive,approverdetail-userfour.
          ELSEIF amountlimit-wflevel = 'L4' OR amountlimit-wflevel = 'l4' OR amountlimit-wflevel = '4'.
            CLEAR: approverdetail-userfive.
          ELSE.
          ENDIF.
        ENDIF.

*        IF baseamount = zfidocumentapproval=>threelack.          " More than 3 lakh → 5 levels
*          CLEAR: approverdetail-userfive.
*        ENDIF.
*        IF baseamount >= zfidocumentapproval=>twolack AND baseamount < zfidocumentapproval=>threelack.         " 2 lakh to 3 lakh → 4 levels
*          CLEAR: approverdetail-userfive,approverdetail-userfour.
*        ENDIF.
*        IF baseamount >= zfidocumentapproval=>onelack AND baseamount < zfidocumentapproval=>twolack.       " 1 lakh to 2 lakh → 3 levels
*          CLEAR: approverdetail-userfive,approverdetail-userfour,approverdetail-userthree.
*        ENDIF.


*        "Fill User Details
        approval[] =  VALUE #( ( user            = COND #( WHEN approverdetail-userone IS NOT INITIAL
                               THEN TEXT-001 && approverdetail-userone )
                                 level           = /isdfps/cl_const_abc_123=>gc_l && /isdfps/cl_const_abc_123=>gc_1 )
                               ( alternativeuser = TEXT-001 && 'NTT_ABAP3' )

                               ( user            = COND #( WHEN approverdetail-usertwo IS NOT INITIAL
                             THEN TEXT-001 && approverdetail-usertwo )
                                 level           = /isdfps/cl_const_abc_123=>gc_l && /isdfps/cl_const_abc_123=>gc_2 )

                               ( user            = COND #( WHEN approverdetail-userthree IS NOT INITIAL
                             THEN TEXT-001 && approverdetail-userthree )
                                 level           = /isdfps/cl_const_abc_123=>gc_l && /isdfps/cl_const_abc_123=>gc_3 )

                               ( user            = COND #( WHEN approverdetail-userfour IS NOT INITIAL
                             THEN TEXT-001 && approverdetail-userfour )
                                 level           = /isdfps/cl_const_abc_123=>gc_l && /isdfps/cl_const_abc_123=>gc_4 )

                               ( user            = COND #( WHEN approverdetail-userfive IS NOT INITIAL
                             THEN TEXT-001 && approverdetail-userfive )
                                 level           = /isdfps/cl_const_abc_123=>gc_l && /isdfps/cl_const_abc_123=>gc_5 ) ).
*
*
*
        DELETE approval[] WHERE user = TEXT-001 && accdocdetail-accountingdoccreatedbyuser.
        DELETE approval[] WHERE user IS INITIAL.
        DELETE approval[] WHERE user = 'US'.
        users =  approval[].
*        sencond-approvals[] = approval[].
*        APPEND sencond TO finals-final .
*        users =   finals-final.
*        APPEND approval TO finals-final[]


*        users-final-approvals[] =  VALUE #( ( user            = COND #( WHEN approverdetail-userone IS NOT INITIAL
*                          THEN TEXT-001 && approverdetail-userone )
*                            level           = /isdfps/cl_const_abc_123=>gc_l && /isdfps/cl_const_abc_123=>gc_1 )
*                          ( alternativeuser = TEXT-001 && 'NTT_ABAP3' )
*
*                          ( user            = COND #( WHEN approverdetail-usertwo IS NOT INITIAL
*                        THEN TEXT-001 && approverdetail-usertwo )
*                            level           = /isdfps/cl_const_abc_123=>gc_l && /isdfps/cl_const_abc_123=>gc_2 )
*
*                          ( user            = COND #( WHEN approverdetail-userthree IS NOT INITIAL
*                        THEN TEXT-001 && approverdetail-userthree )
*                            level           = /isdfps/cl_const_abc_123=>gc_l && /isdfps/cl_const_abc_123=>gc_3 )
*
*                          ( user            = COND #( WHEN approverdetail-userfour IS NOT INITIAL
*                        THEN TEXT-001 && approverdetail-userfour )
*                            level           = /isdfps/cl_const_abc_123=>gc_l && /isdfps/cl_const_abc_123=>gc_4 )
*
*                          ( user            = COND #( WHEN approverdetail-userfive IS NOT INITIAL
*                        THEN TEXT-001 && approverdetail-userfive )
*                            level           = /isdfps/cl_const_abc_123=>gc_l && /isdfps/cl_const_abc_123=>gc_5 ) ).
*
*
*
*        DELETE users-approvals[] WHERE user = TEXT-001 && accdocdetail-accountingdoccreatedbyuser.
*        DELETE users-approvals[] WHERE user IS INITIAL.
*        DELETE users-approvals[] WHERE user = 'US'.


        et_user = users.

*         et_user-approvals[] = users-approvals[].

        zfidocumentapproval=>pending( pendingstatusudp = VALUE #( workflowid   = TEXT-007
                                                                  documentno   = documentno
                                                                  application  = application
                                                                  doccreatedby = accdocdetail-accountingdoccreatedbyuser
                                                                  doccreatedon = accdocdetail-postingdate
                                                                  wflevel      = /isdfps/cl_const_abc_123=>gc_l && /isdfps/cl_const_abc_123=>gc_1
                                                                  status       = TEXT-006 ) ).
*                                                                  userone      = VALUE #( users-approvals[ level = /isdfps/cl_const_abc_123=>gc_l && /isdfps/cl_const_abc_123=>gc_1 ]-user OPTIONAL ) ) ).

      ENDIF.

      "  Populate the Sales Order Change URL
      SELECT SINGLE low FROM tvarvc INTO @DATA(linkdetail) "#EC CI_NOORDER
       WHERE name = @text-002."'ZFIREVIEWLINK' .
   linkdetail = 'https://192.168.82.75:44300/sap/bc/ui2/flp?sap-client=110&sap-language=EN#AccountingDocument-displayDocuments?sap-ui-tech-hint=GUI'.
"'https://192.168.82.75:44300/sap/bc/gui/sap/its/webgui/?%7etransaction=%2aSWF_OBJ_EXEC_CL%20P_CATID%3dBO%3bP_TYPEID%3dBUS2081%3bP_INSTID%3d' && documentno && '2025%3bP_METHOD%3dDISPLAY%3bDYNP_OKCODE%3dONLI&sap-client=110&sap-language=EN#'.
*      IF sy-subrc  = 0.
      DATA(link) = '<a href=' && linkdetail && '>here</a>'.
      CONCATENATE TEXT-003 link INTO tasklink SEPARATED BY space ."'To Review the SO, Please click '
      APPEND tasklink TO tasklinks.
      CLEAR:tasklink.
    ENDIF.
*  ENDIF.


ENDMETHOD.


  METHOD pending.
*----------------------------------------------------------------------*
* Class           ZFIDOCUMENTAPPROVAL
*----------------------------------------------------------------------*
* Title:          Fi Document Approval(Park to post)                   *
* RICEF#:         RSB_FI_R051                                          *
* Transaction:    FB60 , FB65 , FB70 , FB75                            *
*----------------------------------------------------------------------*
* Copyright:      NDBS, Inc.                                           *
* Client:         RSB                                                  *
*----------------------------------------------------------------------*
* Developer:      Saakshi.D                                            *
* Creation Date:  16/07/2025                                           *
* Description:    Fi Document Approval(Park to post)                   *
*----------------------------------------------------------------------*
* Modification History                                                 *
*----------------------------------------------------------------------*
* Modified by:                                                         *
* Date:                                                                *
* Transport:                                                           *
* Description:                                                         *
*----------------------------------------------------------------------*
    " Data Declaration
    DATA:wflogdetails TYPE TABLE OF zfiwflog.

    wflogdetails =   VALUE #( ( workflowid   = pendingstatusudp-workflowid  "TEXT-007
                                documentno   = pendingstatusudp-documentno
                                application  = pendingstatusudp-application
                                doccreatedby = pendingstatusudp-doccreatedby
                                doccreatedon = pendingstatusudp-doccreatedon
                                wflevel      = pendingstatusudp-wflevel
                                status       = pendingstatusudp-status "TEXT-006
                                userone      = pendingstatusudp-userone ) ).

    CHECK wflogdetails IS NOT INITIAL.
    MODIFY zfiwflog FROM TABLE wflogdetails.
    IF sy-subrc  = 0.
      COMMIT WORK.
    ENDIF.
  ENDMETHOD.


  METHOD rejected.
*----------------------------------------------------------------------*
* Class           ZFIDOCUMENTAPPROVAL
*----------------------------------------------------------------------*
* Title:          Fi Document Approval(Park to post)                   *
* RICEF#:         RSB_FI_R051                                          *
* Transaction:    FB60 , FB65 , FB70 , FB75                            *
*----------------------------------------------------------------------*
* Copyright:      NDBS, Inc.                                           *
* Client:         RSB                                                  *
*----------------------------------------------------------------------*
* Developer:      Saakshi.D                                            *
* Creation Date:  16/07/2025                                           *
* Description:    Fi Document Approval(Park to post)                   *
*----------------------------------------------------------------------*
* Modification History                                                 *
*----------------------------------------------------------------------*
* Modified by:                                                         *
* Date:                                                                *
* Transport:                                                           *
* Description:                                                         *
*----------------------------------------------------------------------*
    "  Data Declaration
    TYPES:ty_t_email_ids TYPE TABLE OF ad_smtpadr .
    DATA:wflogdetails            TYPE TABLE OF zfiwflog,
         workitem                TYPE swotobjid,
         workitemid              TYPE swotobjid-objkey,
         simplecontainers        TYPE TABLE OF swr_cont,
         messagelines            TYPE TABLE OF swr_messag,
         messagestructs          TYPE TABLE OF swr_mstruc,
         lsubcontainerborobjects TYPE TABLE OF swr_cont,
         subcontainerallobjects  TYPE TABLE OF swr_cont,
         wkitemid                TYPE swr_struct-workitemid,
         documentid              TYPE sofolenti1-doc_id,
         emailcontent            TYPE bcsy_text,
         emailids_to             TYPE yglobalutility=>tt_email_ids,
         objectcontents          TYPE TABLE OF solisti1.

    CONSTANTS: linebreak TYPE c LENGTH 8 VALUE '<br><br>'.


    IF application = 'Create Inc'.
      application = 'Create Incoming Invoices'.
    ELSEIF application = 'Create Out'.
      application = 'Create Outgoing Invoices'.
    ELSEIF application = 'Manage Cre'.
      application = 'Manage Credit Memo Requests'.
    ELSEIF application = 'Manage Sup'.
      application = 'Manage Supplier Invoices'.
    ENDIF.

    " Rejected Flag
    exit = abap_true.
    "  Get Workitem Details
    CALL FUNCTION 'SWE_WI_GET_FROM_REQUESTER'
      IMPORTING
        requester_workitem   = workitem
        requester_workitemid = workitemid.
    wkitemid             = workitemid.

    "  Get Container Details
    CALL FUNCTION 'SAP_WAPI_READ_CONTAINER'
      EXPORTING
        workitem_id              = wkitemid
        language                 = sy-langu
        user                     = sy-uname
      TABLES
        simple_container         = simplecontainers
        message_lines            = messagelines
        message_struct           = messagestructs
        subcontainer_bor_objects = lsubcontainerborobjects
        subcontainer_all_objects = subcontainerallobjects.

    "  Read the _ATTACH_OBJECTS element
    IF ( line_exists( subcontainerallobjects[ element = TEXT-016 ] ) ). "#EC CI_STDSEQ "_ATTACH_OBJECTS
      documentid = subcontainerallobjects[ element = TEXT-016 ]-value.
    ENDIF.
    "  Read the SOFM Document
    CALL FUNCTION 'SO_DOCUMENT_READ_API1'
      EXPORTING
        document_id                = documentid
      TABLES
        object_content             = objectcontents
      EXCEPTIONS
        document_id_not_exist      = 1
        operation_no_authorization = 2
        x_error                    = 3
        OTHERS                     = 4.
    IF sy-subrc = 0.
      "  Get Remarks
      DATA(remark) = REDUCE #( INIT remarks TYPE string
                                FOR <remarks> IN objectcontents
                                NEXT remarks = COND #( WHEN remarks IS INITIAL
                                                         THEN <remarks>-line
                                                         ELSE remarks && ',' && <remarks>-line ) ).
    ENDIF.

    "  Get Accounting Document Details
    SELECT SINGLE accountingdocument,
                  accountingdoccreatedbyuser FROM i_journalentry
                                             INTO @DATA(accdocdetail)
                                             WHERE accountingdocument = @documentno.

    IF sy-subrc  = 0.
      "  Fetch privious workitem id
      SELECT SINGLE top_wi_id            "#EC CI_NOORDER or "#EC WARNOK
                   FROM swwwihead "wi_cruser
                          INTO @DATA(priviousid)
                          WHERE wi_id = @workitemid.
      IF sy-subrc  = 0.
        "  Get user for current workitem id for without levels
        SELECT SINGLE wi_aagent FROM swwwihead "#EC CI_NOORDER or "#EC WARNOK
                         INTO @rejector
                         WHERE top_wi_id = @priviousid
                            AND wi_rh_task = 'TS99900004'.

        IF sy-subrc  <> 0.
          SELECT SINGLE wi_aagent FROM swwwihead "#EC CI_NOORDER or "#EC WARNOK
                         INTO @rejector
                         WHERE top_wi_id = @priviousid
                            AND wi_rh_task = 'TS99900009'.
        ENDIF.

        DATA(workitemno) = workitemid - 4.
        SELECT SINGLE top_wi_id "wi_cruser
                    FROM swwwihead "wi_cruser
             INTO  @DATA(topid)
                            WHERE wi_id = @workitemno.
        IF sy-subrc  = 0.
          SELECT SINGLE wi_creator "wi_cruser
               FROM swwwihead "wi_cruser
               INTO  @initiator
                              WHERE wi_id = @topid.

        ENDIF.
        IF rejector IS NOT INITIAL.
          data(realrejector) = rejector.
          SELECT application,            "#EC CI_NOORDER or "#EC WARNOK
                 userone,
                 usertwo,
                 userthree,
                 userfour,
                 userfive,
                 altuserone,
                 altusertwo,
                 altuserthree,
                 altuserfour,
                 altuserfive FROM zfidocusers INTO TABLE @DATA(rejectorsdetails)
                                WHERE application = @application.

          IF sy-subrc  = 0.
            SELECT SINGLE application,   "#EC CI_NOORDER or "#EC WARNOK
                                    userone,
                                    usertwo,
                                    userthree,
                                    userfour,
                                    userfive,
                                    altuserone,
                                    altusertwo,
                                    altuserthree,
                                    altuserfour,
                                    altuserfive FROM @rejectorsdetails AS rejectorsdetails
                                            WHERE application LIKE @application
                                              AND ( userone = @rejector
                                               OR usertwo   = @rejector
                                               OR userthree = @rejector
                                               OR userfour  = @rejector
                                               OR userfive  = @rejector ) INTO @DATA(rejectors).
            IF sy-subrc <> 0.
              SELECT SINGLE application, "#EC CI_NOORDER or "#EC WARNOK
                                 userone,
                                 usertwo,
                                 userthree,
                                 userfour,
                                 userfive,
                                 altuserone,
                                 altusertwo,
                                 altuserthree,
                                 altuserfour,
                                 altuserfive FROM @rejectorsdetails AS rejectorsdetails
                                         WHERE application LIKE @application
                                           AND ( altuserone = @rejector
                                            OR altusertwo   = @rejector
                                            OR altuserthree = @rejector
                                            OR altuserfour  = @rejector
                                            OR altuserfive  = @rejector )  INTO @rejectors.

              DATA(altrejector)     = COND #( WHEN rejectors-altuserfive = rejector
                                         THEN rejectors-userfive
                                         WHEN rejectors-altuserfour  = rejector
                                         THEN rejectors-userfour
                                         WHEN rejectors-altuserthree  = rejector
                                         THEN rejectors-userthree
                                         WHEN rejectors-altusertwo  = rejector
                                         THEN rejectors-usertwo
                                         WHEN rejectors-altuserone  = rejector
                                         THEN rejectors-userone  ).

            ELSE.
              altrejector     = COND #( WHEN rejectors-userfive = rejector
                                      THEN rejectors-altuserfive
                                      WHEN rejectors-userfour  = rejector
                                      THEN rejectors-altuserfour
                                      WHEN rejectors-userthree  = rejector
                                      THEN rejectors-altuserthree
                                      WHEN rejectors-usertwo  = rejector
                                      THEN rejectors-altusertwo
                                      WHEN rejectors-userone   = rejector
                                      THEN rejectors-altuserone ).

            ENDIF.
          ENDIF.



*017 Hi
*018  Document
*019  has been rejected. Please check the remarks.
*020  Thanks and Regards
select SINGLE MC_NAMEFIR from V_USERNAME into @data(approvername) where BNAME = @realrejector.
  if sy-subrc  = 0.
    else.
      endif.
          emailcontent =   VALUE #( ( line = | { TEXT-017 }| )
                                    ( line = linebreak )
                                    ( line = |{ TEXT-018 } { | | }{ documentno } { | | } { TEXT-019 } { | | } { approvername }. { TEXT-025 }| )
                                    ( line = linebreak )
                                    ( line = TEXT-020 ) ).

          SELECT a~smtp_addr FROM adr6 AS a INNER JOIN usr21 ON usr21~addrnumber = a~addrnumber
                                                 AND usr21~persnumber = a~persnumber
                                            INTO TABLE @emailids_to WHERE usr21~bname IN ( @rejector , @altrejector ).
          IF sy-subrc  = 0.
          ELSE.
          ENDIF.

          SELECT SINGLE * FROM zfiwflog INTO @DATA(approvallogdetail)
                                  WHERE documentno = @documentno
                                    AND application = @application.

*          IF sy-subrc  = 0 .
          IF approvallogdetail IS NOT INITIAL .
            IF approvallogdetail-usertwo IS NOT INITIAL.
              SELECT a~smtp_addr FROM adr6 AS a INNER JOIN usr21 ON usr21~addrnumber = a~addrnumber
                                                     AND usr21~persnumber = a~persnumber
                                                APPENDING TABLE @emailids_to WHERE usr21~bname IN ( @approvallogdetail-usertwo, @approvallogdetail-altusertwo ).
            ENDIF.
            IF approvallogdetail-userone   IS NOT INITIAL.
              SELECT a~smtp_addr FROM adr6 AS a INNER JOIN usr21 ON usr21~addrnumber = a~addrnumber
                                                     AND usr21~persnumber = a~persnumber
                                                APPENDING TABLE @emailids_to WHERE usr21~bname IN ( @approvallogdetail-userone, @approvallogdetail-altuserone ).
            ENDIF.
            IF approvallogdetail-userthree   IS NOT INITIAL.
              SELECT a~smtp_addr FROM adr6 AS a INNER JOIN usr21 ON usr21~addrnumber = a~addrnumber
                                                     AND usr21~persnumber = a~persnumber
                                                APPENDING TABLE @emailids_to WHERE usr21~bname IN ( @approvallogdetail-userthree, @approvallogdetail-altuserthree ).
            ENDIF.

          ENDIF.
          IF initiator IS NOT INITIAL.
            SELECT a~smtp_addr FROM adr6 AS a INNER JOIN usr21 ON usr21~addrnumber = a~addrnumber
                                                   AND usr21~persnumber = a~persnumber
                                              APPENDING TABLE @emailids_to WHERE usr21~bname = @initiator.
          ENDIF.

          DELETE ADJACENT DUPLICATES FROM emailids_to.

          IF  approvallogdetail-wflevel = 'L1' .
            approvallogdetail-status = TEXT-005 .
            approvallogdetail-approvaldateu1 = sy-datum .
            approvallogdetail-remarksu1 = remark .

            APPEND approvallogdetail TO wflogdetails.

          ELSEIF approvallogdetail-wflevel = 'L2'.
            approvallogdetail-status2 = TEXT-005 .
            approvallogdetail-approvaldateu2 = sy-datum .
            approvallogdetail-remarksu2 = remark .

            APPEND approvallogdetail TO wflogdetails.

          ELSEIF approvallogdetail-wflevel = 'L3'.
            approvallogdetail-status3 = TEXT-005 .
            approvallogdetail-approvaldateu3 = sy-datum .
            approvallogdetail-remarksu3 = remark .

            APPEND approvallogdetail TO wflogdetails.

          ELSEIF approvallogdetail-wflevel = 'L4'.
            approvallogdetail-status4 = TEXT-005 .
            approvallogdetail-approvaldateu4 = sy-datum .
            approvallogdetail-remarksu4 = remark .

            APPEND approvallogdetail TO wflogdetails.


          ELSEIF approvallogdetail-wflevel = 'L5' .

            approvallogdetail-status5 = TEXT-005 .
            approvallogdetail-approvaldateu5 = sy-datum .
            approvallogdetail-remarksu5 = remark .

            APPEND approvallogdetail TO wflogdetails.
          ENDIF.
          "Trigger Email
          TRY.
              yglobalutility=>sendemail(
                EXPORTING
                  sender      = 'SAP.NOTIFICATION@RSBGLOBAL.COM'           " E-Mail Address
                  subject     = 'Document' && | | && documentno && | | && 'Rejected'          " Maintenance Notification Alert for Notification number
                  emailids_to = emailids_to                                  " TO
                  email_body  = emailcontent                                 " Email Body                                             " CC
                IMPORTING
                  sucess_flag = DATA(flag)
                  message     = DATA(message) ).
            CATCH cx_root.
          ENDTRY.
        ENDIF.
      ENDIF.
    ENDIF.

    IF wflogdetails IS NOT INITIAL.
      MODIFY zfiwflog FROM TABLE wflogdetails.
      IF sy-subrc  = 0.
        COMMIT WORK.
      ENDIF.








































***          IF  approvallogdetail-wflevel = 'L1' AND approvallogdetail-status <> TEXT-006.
***
***            wflogdetails =  VALUE #( ( workflowid     = approvallogdetail-workflowid
***                                       documentno     = approvallogdetail-documentno
***                                       application    = approvallogdetail-application
***                                       doccreatedby   = approvallogdetail-doccreatedby
***                                       doccreatedon   = approvallogdetail-doccreatedon
***                                       wflevel        = 'L2'
***                                       status         = TEXT-005     "Rejected
***                                       userone        = approvallogdetail-userone
***                                       approvaldateu1 = approvallogdetail-approvaldateu1
***                                       remarksu1      = approvallogdetail-remarksu1
***                                       usertwo        = rejector
***                                       approvaldateu2 = sy-datum
***                                       remarksu2      = remark ) ).
***
***            "Trigger Email
***
***            TRY.
***                yglobalutility=>sendemail(
***                  EXPORTING
***                    sender      = 'SAP.NOTIFICATION@RSBGLOBAL.COM'           " E-Mail Address
***                    subject     = 'Document' && | | && documentno && | | && 'Rejected'          " Maintenance Notification Alert for Notification number
***                    emailids_to = emailids_to                                  " TO
***                    email_body  = emailcontent                                 " Email Body                                             " CC
***                  IMPORTING
***                    sucess_flag = DATA(flag)
***                    message     = DATA(message) ).
***              CATCH cx_root.
***            ENDTRY.
***
***          ELSEIF approvallogdetail-wflevel = 'L2'.
***            wflogdetails =  VALUE #( ( workflowid     = approvallogdetail-workflowid
***                                       documentno     = approvallogdetail-documentno
***                                       application    = approvallogdetail-application
***                                       doccreatedby   = approvallogdetail-doccreatedby
***                                       doccreatedon   = approvallogdetail-doccreatedon
***                                       wflevel        = 'L3'
***                                       status         = TEXT-005      "Rejected
***                                       userone        = approvallogdetail-userone
***                                       approvaldateu1 = approvallogdetail-approvaldateu1
***                                       remarksu1      = approvallogdetail-remarksu1
***                                       usertwo        = approvallogdetail-usertwo
***                                       approvaldateu2 = approvallogdetail-approvaldateu2
***                                       remarksu2      = approvallogdetail-remarksu2
***                                       userthree      = rejector
***                                       approvaldateu3 = sy-datum
***                                       remarksu3      = remark ) ).
***            TRY.
***                yglobalutility=>sendemail(
***                  EXPORTING
***                    sender      = 'SAP.NOTIFICATION@RSBGLOBAL.COM'           " E-Mail Address
***                    subject     = 'Document' && | | && documentno && | | && 'Rejected'          " Maintenance Notification Alert for Notification number
***                    emailids_to = emailids_to                                  " TO
***                    email_body  = emailcontent                                 " Email Body                                             " CC
***                  IMPORTING
***                    sucess_flag = flag
***                    message     = message ).
***              CATCH cx_root.
***            ENDTRY.
***
***          ELSEIF approvallogdetail-wflevel = 'L3'.
***            wflogdetails =  VALUE #( ( workflowid     = approvallogdetail-workflowid
***                                       documentno     = approvallogdetail-documentno
***                                       application    = approvallogdetail-application
***                                       doccreatedby   = approvallogdetail-doccreatedby
***                                       doccreatedon   = approvallogdetail-doccreatedon
***                                       wflevel        = 'L4'
***                                       status         = TEXT-005      "Rejected
***                                       userone        = approvallogdetail-userone
***                                       approvaldateu1 = approvallogdetail-approvaldateu1
***                                       remarksu1      = approvallogdetail-remarksu1
***                                       usertwo        = approvallogdetail-usertwo
***                                       approvaldateu2 = approvallogdetail-approvaldateu2
***                                       remarksu2      = approvallogdetail-remarksu2
***                                       userthree      = approvallogdetail-userthree
***                                       approvaldateu3 = approvallogdetail-approvaldateu3
***                                       remarksu3      = approvallogdetail-remarksu3
***                                       userfour       = rejector
***                                       approvaldateu4 = sy-datum
***                                       remarksu4      = remark ) ).
***
***            TRY.
***                yglobalutility=>sendemail(
***                  EXPORTING
***                    sender      = 'SAP.NOTIFICATION@RSBGLOBAL.COM'           " E-Mail Address
***                    subject     = 'Document' && | | && documentno && | | && 'Rejected'          " Maintenance Notification Alert for Notification number
***                    emailids_to = emailids_to                                  " TO
***                    email_body  = emailcontent                                 " Email Body                                             " CC
***                  IMPORTING
***                    sucess_flag = flag
***                    message     = message ).
***              CATCH cx_root.
***            ENDTRY.
***
***          ELSEIF approvallogdetail-wflevel = 'L4'.
***            wflogdetails =  VALUE #( ( workflowid     = approvallogdetail-workflowid
***                                       documentno     = approvallogdetail-documentno
***                                       application    = approvallogdetail-application
***                                       doccreatedby   = approvallogdetail-doccreatedby
***                                       doccreatedon   = approvallogdetail-doccreatedon
***                                       wflevel        = 'L5'
***                                       status         = TEXT-005      "Rejected
***                                       userone        = approvallogdetail-userone
***                                       approvaldateu1 = approvallogdetail-approvaldateu1
***                                       remarksu1      = approvallogdetail-remarksu1
***                                       usertwo        = approvallogdetail-usertwo
***                                       approvaldateu2 = approvallogdetail-approvaldateu2
***                                       remarksu2      = approvallogdetail-remarksu2
***                                       userthree      = approvallogdetail-userthree
***                                       approvaldateu3 = approvallogdetail-approvaldateu3
***                                       remarksu3      = approvallogdetail-remarksu3
***                                       userfour       = approvallogdetail-userfour
***                                       approvaldateu4 = approvallogdetail-approvaldateu4
***                                       remarksu4      = approvallogdetail-remarksu4
***                                       userfive       = rejector
***                                       approvaldateu5 = sy-datum
***                                       remarksu5      = remark ) ).
***
***
***            TRY.
***                yglobalutility=>sendemail(
***                  EXPORTING
***                    sender      = 'SAP.NOTIFICATION@RSBGLOBAL.COM'           " E-Mail Address
***                    subject     = 'Document' && | | && documentno && | | && 'Rejected'          " Maintenance Notification Alert for Notification number
***                    emailids_to = emailids_to                                  " TO
***                    email_body  = emailcontent                                 " Email Body                                             " CC
***                  IMPORTING
***                    sucess_flag = flag
***                    message     = message ).
***              CATCH cx_root.
***            ENDTRY.
***
***          ELSEIF approvallogdetail-wflevel = 'L1' AND approvallogdetail-status = TEXT-006. "PENDING
***
***            wflogdetails =   VALUE #( ( workflowid     = TEXT-007 "WORKFLOWID
***                                        documentno     = approvallogdetail-documentno
***                                        application    = approvallogdetail-application
***                                        doccreatedby   = approvallogdetail-doccreatedby
***                                        doccreatedon   = approvallogdetail-doccreatedon
***                                        wflevel        = 'L1'
***                                        status         = TEXT-005    "Rejected
***                                        userone        = rejector
***                                        approvaldateu1 = sy-datum
***                                        remarksu1      = remark ) ).
***            TRY.
***                yglobalutility=>sendemail(
***                  EXPORTING
***                    sender      = 'SAP.NOTIFICATION@RSBGLOBAL.COM'           " E-Mail Address
***                    subject     = 'Document' && | | && documentno && | | && 'Rejected'          " Maintenance Notification Alert for Notification number
***                    emailids_to = emailids_to                                  " TO
***                    email_body  = emailcontent                                 " Email Body                                             " CC
***                  IMPORTING
***                    sucess_flag = flag
***                    message     = message ).
***              CATCH cx_root.
***            ENDTRY.
***
***
***
***          ENDIF.
***        ENDIF.
***      ENDIF.
***    ENDIF.
***
***    IF wflogdetails IS NOT INITIAL.
***      MODIFY zfiwflog FROM TABLE wflogdetails.
***      IF sy-subrc  = 0.
***        COMMIT WORK.
***      ENDIF.

*      SELECT  bname,
*              persnumber FROM usr21 INTO TABLE @DATA(lt_personno) WHERE bname = @ev_initiator.
*      IF  sy-subrc  = 0.
*        SELECT  adr6~smtp_addr FROM adr6
*                               INNER JOIN @lt_personno AS lt_personno
*                               ON lt_personno~persnumber = adr6~persnumber
*                               INTO TABLE @lt_email_ids_to.
*      ENDIF.
*
*"  Fetching from Mail Address
*      SELECT SINGLE  emailaddress FROM i_addressemailaddress WITH PRIVILEGED ACCESS
*                                  INNER JOIN i_plant ON i_plant~plant = @ls_so_details-werks
*                                  AND i_plant~addressid = i_addressemailaddress~addressid
*                                  AND i_addressemailaddress~ordinalnumber = @/isdfps/cl_const_abc_123=>gc_3
*                                  INTO @DATA(lv_from_mail).
*      IF sy-subrc  = 0.
*        "do nothing
*      ELSE.
*      ENDIF.
*
*      lv_textid = TEXT-009."  ST
*      lv_textobj = TEXT-010."  TEXT
*      lv_name = TEXT-011."ZSD_001_SO_APPROVAL
*"  get mail content
*      data(lt_mailbody) = zcl_global_utilities=>read_text_tab( is_textdetails = VALUE #(  textid  = lv_textid
*                                                                                          name    = lv_name
*                                                                                          textobj = lv_textobj ) ).
*      LOOP AT lt_mailbody ASSIGNING FIELD-SYMBOL(<fs_mailbody>).
*        REPLACE ALL OCCURRENCES OF '&INITIATOR&' IN <fs_mailbody> WITH ev_initiator.
*        REPLACE ALL OCCURRENCES OF '&rejector&' IN <fs_mailbody> WITH rejector.
*        REPLACE ALL OCCURRENCES OF '&REMARKS&'  IN <fs_mailbody> WITH lv_remarks.
*        APPEND VALUE #( line = <fs_mailbody> ) TO lt_email_body .
*      ENDLOOP.
*"  Subject
*      lv_subject = 'Sales Order' && | | && iv_salesorder && ' Rejected'.
*"  Send Email to initiator to notify approval and rejection
*
*      TRY.
*          zcl_global_utilities=>sendemail(
*            EXPORTING
*              iv_email_sender  = lv_from_mail                 " E-Mail Address
*              iv_email_subject = lv_subject                   " Short description of contents
*              it_email_ids_to  = lt_email_ids_to
*              it_email_body    = lt_email_body                " Text Table
*            IMPORTING
*              ev_sucess_flag   = DATA(ev_sucess_flag)         " Boolean Variable (X = True, - = False, Space = Unknown)
*              ev_message       = DATA(ev_message)
*          ).
*        CATCH cx_root.
*      ENDTRY.
    ENDIF.

  ENDMETHOD.
ENDCLASS.
***********************************************************************************************************************************************

class ZKRDOCUMENTAPPROVAL definition
  public
  final
  create public .

public section.

  interfaces BI_OBJECT .
  interfaces BI_PERSISTENT .
  interfaces IF_WORKFLOW .
protected section.
private section.

  methods GETUSER .
  methods APPRVED .
  methods REJECTED .
  methods PENDING .
  methods DOCUMENTRECHECK .
ENDCLASS.



CLASS ZKRDOCUMENTAPPROVAL IMPLEMENTATION.


  method APPRVED.
  endmethod.


  method DOCUMENTRECHECK.
  endmethod.


  method GETUSER.
  endmethod.


  method PENDING.
  endmethod.


  method REJECTED.
  endmethod.
ENDCLASS.
******************************************************************************************************************************************
