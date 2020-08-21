
- **Push lên Repositories không cần đăng nhập**

```
remote set-url origin https://[username]:[password]@github.com/ducanh847/shop.git 
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
