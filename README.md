
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

