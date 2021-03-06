---
auto_validation: true
title: Create Authorization in SAP Cloud Platform ABAP environment
description: Create IAM Apps, services and catalogs for authorization in the SAP Cloud Platform ABAP environment.
primary_tag: products>sap-cloud-platform--abap-environment
tags: [  tutorial>beginner, topic>abap-development, products>sap-cloud-platform, tutorial>license]
time: 20
---

## Prerequisites  
  - SAP Cloud Platform ABAP Environment user
  - ADT version 2.96 or higher

## Details
### You will learn
  - How to create authorization fields
  - How to create access control
  - How to edit authorization default values
  - How to create IAM Apps and services
  - How to create business catalog

In this tutorial, wherever `XXX` appears, use a number (e.g. `000`).

---


[ACCORDION-BEGIN [Step 1: ](Create authorization field)]
1. Right-click on **`Z_ROOM_XXX`**, select the menu path **New** > **Other ABAP Repository Object**.

      ![Create authorization field](field.png)

2. Search for **Authorization Field**, select it and click **Next>**.

    ![Create authorization field](field2.png)

2. Create your **authorization field**:
     - Name: **`Z_LOCAFXXX`**

     Click **Next>**.

    ![Create authorization field](field3.png)

3. Click **Finish**.

    ![Create authorization field](field4.png)

4. Edit your authorization field:
      - Data Element: **`Z_LOCA_DTEL_XXX`**

    Save and activate.

    ![Create authorization field](field5.png)

[DONE]
[ACCORDION-END]


[ACCORDION-BEGIN [Step 2: ](Create authorization object)]
1. Right-click on **`Z_ROOM_XXX`**, select the menu path **New** > **Other ABAP Repository Object**.

      ![Create authorization object](object.png)

2. Search for **Authorization Object**, select it and click **Next>**.

    ![Create authorization object](object2.png)

2.  Create your **authorization object**:
       - Name: **`Z_LOCAOXXX`**
       - Description: **`Location`**

       Click **Next>**.

       ![Create authorization object](object3.png)

3. Click **Finish**.

      ![Create authorization object](object4.png)

4. Edit your authorization object and save it. The description and access category will appear then.

      ![Create authorization object](object5.png)

5. Add `WDF` as value to authorization field `Z_LOCAFXXX`.

      ![Create authorization object](object6.png)

[DONE] 
[ACCORDION-END]

[ACCORDION-BEGIN [Step 3: ](Create access control)]
  1. Right-click on **`Z_ROOM_XXX`**, select the menu path **New** > **Other ABAP Repository Object**.

      ![Create Access Control](access.png)

  2. Search for **Access Control**, select it and click **Next>**.

      ![Create Access Control](access2.png)

  3.  Create your **service definition:**
     - Name: **`Z_I_ROOM_XXX`**
     - Description: **`Room`**

     Click **Next>**.

      ![Create Access Control](access3.png)

  4. Click **Next>**.

      ![Create Access Control](access4.png)

  5. Select **Define Role with PFCG Aspect** and click **Finish**.

      ![Create Access Control](access5.png)

  6. Edit your service definition:
    ```ABAP
    @EndUserText.label: 'Room'
    @MappingRole: true
    define role Z_I_Room_XXX
    {
      grant
        select
            on
                Z_I_ROOM_XXX
                    where
                        (location) = aspect pfcg_auth(Z_LOCAOXXX, Z_LOCAFXXX, ACTVT = '03');  
    }
    ```
    Save and activate.

[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 4: ](Enhance behavior)]
Switch to your behavior implementation, click `CTRL + F` and search for method validate. Edit following as your validate method.
```ABAP
     METHOD validate.
        AUTHORITY-CHECK OBJECT 'Z_LOCAOXXX' ID 'ACTVT' FIELD iv_action ID 'Z_LOCAFXXX' FIELD is_room-location.
        IF sy-subrc <> 0.
          rv_message = 'Not authorized'.
        ENDIF.
    ENDMETHOD.
```

Save and activate.

[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 5: ](Edit authorization default values)]
  1. Open your default authorization value `Z_I_ROOM_BND_XXX`.

      ![Edit authorization default values](default.png)

  2. Copy `Z_LOCAOXXX` as an authorization and click on your error message on the top right corner. Check your result.

      ![Edit authorization default values](default2.png)

  3. Set your default values for objects `S_SERVICE` and  `Z_LOCAOXXX`.


[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 6: ](Create IAM app & add service)]
  1. Right-click on **`Z_ROOM_XXX`**, select the menu path **New** > **Other ABAP Repository Object**.

      ![Create Access Control](app.png)

  2. Search for **IAM App**, select it and click **Next>**.

      ![Create Access Control](app2.png)

  3.  Create your **IAM App:**
     - Name: **`Z_ROOM_XXX`**
     - Description: **`Room`**

     Click **Next>**.

      ![Create Access Control](app3.png)

  4. Click **Finish**.

      ![Create Access Control](app4.png)

  5. Select **Services**.

      ![Create Access Control](app5.png)

  6. Add new services.

      ![Create Access Control](app6.png)

  7. Find your service:
    - Service Type: `OData V2`
    - Service Name: `Z_I_ROOM_BND_XXX_0001`

    Add `_0001` to your service name to find it.
    Click **OK**.

      ![Create Access Control](app7.png)

  8. Click **Authorizations**.

      ![Create Access Control](app8.png)

  9. Add new authorization objects.

      ![Create Access Control](app9.png)

 10. Search `Z_LOCAOXXX` and click OK.

      ![Create Access Control](app10.png)

 11. Check your result.

      ![Create Access Control](app11.png)

 12. Select `Authorization_0001` and click **Edit...**.

      ![Create Access Control](app12.png)

 13. Check all field values and click **OK**.

      ![Create Access Control](app13.png)

 14. Add `WDF` to `Z_LOCAFXXX`.
     Save and activate.

      ![Create Access Control](app14.png)

[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 7: ](Create business catalog & add IAM app)]
  1. Right-click on **`Z_ROOM_XXX`**, select the menu path **New** > **Other ABAP Repository Object**.

      ![Create Access Control](catalog.png)

  2. Search for **Business Catalog**, select it and click **Next>**.

      ![Create Access Control](catalog2.png)

  3.  Create your **business catalog:**
     - Name: **`Z_ROOM_BC_XXX`**
     - Description: **`Room`**

     Click **Next>**.

      ![Create Access Control](catalog3.png)

  4. Click **Finish**.

      ![Create Access Control](catalog4.png)

  5. Select **Apps**.

      ![Create Access Control](catalog5.png)

  6. Add new Apps.

      ![Create Access Control](catalog6.png)

  7. Add your App:
    - App ID: `Z_ROOM_XXX_EXT`
    - Assignment ID: `Z_ROOM_BC_XXX_0001`

    Click **Next>**.

      ![Create Access Control](catalog7.png)

  8.  Click **Finish**.

      ![Create Access Control](catalog8.png)

  9. Click **Publish Locally**

      ![Create Access Control](catalog9.png)

[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 8: ](Test yourself)]

[VALIDATE_1]
[ACCORDION-END]
