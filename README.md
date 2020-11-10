
- **Push lên Repositories không cần đăng nhập**

```
remote set-url origin https://[username]:[password]@github.com/[Link_Repositories] 
```

- **Sắp xếp file json theo anphabe**

```
var unordered = { 
  "Forgot Password": "Forgot Password",
  "ARR": "ARR", 
  "Equipment Maintenance": "Equipment Maintenance", 
  "Department": "Department", 
  "Access": "Access", 
  "Accident": "Accident" 
} 
var ordered = {};
Object.keys(unordered).sort().forEach(function (key) {
    ordered[key] = unordered[key];
});
console.log(JSON.stringify(ordered))
```

- **Thêm property vào object**

```
var Obj = {
  "studentId": "15130008",
  "name": "Phan Duc Anh",
  "dateOfBirth": "08/04/1997"
}
Object.assign(Obj, { address: "TP.HCM" });
console.log(Obj)
```

- **Format number tiền tệ**

```
const formatNumber = (num) => {
  return num.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, "$1,");
};
console.log(formatNumber(1250000))
```
- **CRUD React-hook**

```
const [listObj, setListObj] = useState();
var Obj = {
  "studentId": "15130008",
  "name": "Phan Duc Anh",
  "dateOfBirth": "08/04/1997"
}
// add new item to list
setListObj((listObj) => [...listObj, Obj]);
// edit item
var indexObj = listObj.findIndex((x) => x.studentId === [id]);
if(indexObj !== -1){
  listObj[indexObj].studentId = [newID];
  listObj[indexObj].name = [newName];
  listObj[indexObj].dateOfBirth = [newDateOfBirth];
}
// delete item
var listAfterRemove = listobj.filter((x) => x.studentId !== [id]);
setListObj(listAfterRemove);

```
- **Search Appointment**
```
/****** Object:  StoredProcedure [dbo].[usp_SearchAppointment]    Script Date: 10/28/2020 3:00:48 PM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
-- =============================================
-- Author:      <Author, , Name>
-- Create Date: <Create Date, , >
-- Description: <Description, , >
-- =============================================
/****** Object:  StoredProcedure [dbo].[usp_SearchAppointment]    Script Date: 11/10/2020 1:48:50 PM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
-- =============================================
-- Author:      <Author, , Name>
-- Create Date: <Create Date, , >
-- Description: <Description, , >
-- =============================================
ALTER PROCEDURE [dbo].[usp_SearchAppointment]
(
    @FirstName VARCHAR(200),
	@LastName VARCHAR(200),
	@Phone VARCHAR(20),
	@Email VARCHAR(50),
	@DateOfBirth DATE NULL,
	@Covid VARCHAR(50),
	@IsCollector BIT NULL,
	@Skip INT = 0,
	@Take INT = 20
)
AS
BEGIN
   SET NOCOUNT ON
	--PRINT STR(@Skip);
	--PRINT STR(@DateOfBirth);
	SELECT p.Patient_Id into #appId
	      FROM dbo.Patient AS p
		   WHERE (ISNULL(@FirstName,'') = '' OR LOWER(p.Patient_First) LIKE LOWER(@FirstName) + '%')
		   AND (ISNULL(@LastName,'') = '' OR LOWER(p.Patient_Last) LIKE LOWER(@LastName) + '%')
		   AND (ISNULL(@Phone,'') = '' OR p.Patient_Phone LIKE @Phone + '%' OR p.Patient_Mobile LIKE @Phone + '%')
		  /* AND (ISNULL(@Phone,'') = '' OR REPLACE(REPLACE(REPLACE(REPLACE(p.Patient_Phone,'-', '' ),'(', ''), ')', ''), ' ', '') LIKE '%' + @Phone + '%' 
		                               OR REPLACE(REPLACE(REPLACE(REPLACE(p.Patient_Mobile,'-', '' ),'(', ''), ')', ''), ' ', '') LIKE '%' + @Phone + '%')*/
		   AND (ISNULL(@Email,'') = '' OR LOWER(p.Patient_Email) LIKE '%' + LOWER(@Email) + '%')
		   AND (@DateOfBirth IS NULL OR TRY_CONVERT(DATE, p.Patient_DOB) = @DateOfBirth)
		   AND (ISNULL(@Covid, '') = '' OR LOWER(p.COVID) LIKE LOWER(@Covid) + '%')
		   AND ISNULL(p.[GUID],'') <> ''
		   AND p.IsDeleted = 0;

    SELECT COUNT(1) FROM #appId;
	--SELECT * FROM #appId
	SELECT
		   p.Patient_First as PatientFirst,
		   p.Patient_Last as PatientLast,
		   p.Patient_Email as PatientEmail,
		   p.Patient_Phone as PatientPhone,
		   p.Patient_DOB as PatientDob,
		   p.GUID as PatientGUID,
		   p.Patient_Middle as MiddleName,
           p.Patient_Suffix as Suffix,
		   p.COVID as Cov,
		   p.Patient_Street_Address as StreetAddress,
		   p.Patient_Street_Address_2 as StreetAddress2,
		   p.Form_Date as SampleSubmissionDate,
		   p.Sample_Date as SampleCollectionDate,
		   p.Patient_Mobile as PatientMobile,
		   IIF(ISNULL(p.[Username], 0) > 0, 1, 0) AS IsRegister
	       FROM dbo.Patient AS p 
		 WHERE (SELECT COUNT(1) FROM #appId) > 0 AND p.Patient_Id IN (SELECT Patient_Id FROM #appId)
		 ORDER BY p.Patient_Id DESC OFFSET @Skip ROWS FETCH NEXT @Take ROWS ONLY;  
 

	DROP TABLE #appId;
END

```

- **Search Patient**
```
/****** Object:  StoredProcedure [dbo].[usp_SearchPatient]    Script Date: 11/10/2020 1:55:43 PM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
ALTER PROCEDURE [dbo].[usp_SearchPatient]
(
    @FirstName VARCHAR(200),
	@LastName VARCHAR(200),
	@Phone VARCHAR(20),
	@Email VARCHAR(50),
	@DateOfBirth DATE NULL,
	@Covid VARCHAR(50),
	@Skip INT = 0,
	@Take INT = 20,
	@SiteCode VARCHAR(200)
)
AS
BEGIN
    SET NOCOUNT ON
	PRINT STR(@Skip);
	PRINT STR(@Take);
	SELECT p.Patient_Id into #patientId
	       FROM dbo.Patient AS p
		   WHERE (ISNULL(@FirstName,'') = '' OR p.Patient_First LIKE @FirstName + '%')
		   AND (ISNULL(@LastName,'') = '' OR p.Patient_Last LIKE @LastName + '%')
		   AND (ISNULL(@Phone,'') = '' OR p.Patient_Phone LIKE @Phone + '%' OR p.Patient_Mobile LIKE @Phone + '%')
		   /*AND (ISNULL(@Phone,'') = '' OR REPLACE(REPLACE(REPLACE(REPLACE(p.Patient_Phone,'-', '' ),'(', ''), ')', ''), ' ', '') LIKE '%' + @Phone + '%' 
		                               OR REPLACE(REPLACE(REPLACE(REPLACE(p.Patient_Mobile,'-', '' ),'(', ''), ')', ''), ' ', '') LIKE '%' + @Phone + '%')*/
		   AND (ISNULL(@Email,'') = '' OR p.Patient_Email LIKE '%' + @Email + '%')
		   AND (@DateOfBirth IS NULL OR TRY_CONVERT(DATE, p.Patient_DOB) = @DateOfBirth)
		   AND (ISNULL(@Covid, '') = '' OR p.COVID LIKE @Covid + '%')
		   AND ISNULL(p.[GUID],'') <> ''
		   AND ISNULL(@SiteCode,'') = p.Site_Code;

    SELECT COUNT(1) FROM #patientId;

	SELECT p.Guid
	     , p.Patient_First AS FirstName
	     , p.Patient_Middle AS MiddleName
		 , p.Patient_Last AS LastName
		 , p.Patient_DOB AS DateOfBirth
		 , p.Patient_Phone AS Phone
		 , p.Patient_Email AS Email
		 , p.Patient_Street_Address AS StreetAddress
		 , p.Sample_Date AS SampleCollectionDate
		 , p.Form_Date AS SampleSubmissionDate
		 , p.Specimen_ID AS TRFNumber
		 , p.Patient_Street_Address_2 AS AptUnitSuite
		 , p.Patient_Suffix AS Suffix
		 , p.COVID AS Covid
		 , p.Patient_Mobile AS PatientMobile
		 , IIF(ISNULL(p.[Username], 0) > 0, 1, 0) AS IsRegister
		 FROM dbo.Patient AS p
		 WHERE (SELECT COUNT(1) FROM #patientId) > 0 AND p.Patient_Id IN (SELECT Patient_Id FROM #patientId) AND IsDeleted = 0
		 ORDER BY p.Patient_Id DESC OFFSET @Skip ROWS FETCH NEXT @Take ROWS ONLY;  

	DROP TABLE #patientId;
END






```



