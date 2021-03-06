---
title: Hapus
layout: page
---
## Hapus catatan

**PERINGATAN** Saat menghapus catatan, anda harus memastikan bidang utama itu memiliki nilai, dan GORM akan menggunakan kunci utama untuk menghapus catatan, jika bidang utama kosong, GORM akan menghapus semua catatan untuk model tersebut

```go
// Delete an existing record
db.Delete(&email)
//// DELETE from emails where id=10;

// Add extra SQL option for deleting SQL
db.Set("gorm:delete_option", "OPTION (OPTIMIZE FOR UNKNOWN)").Delete(&email)
//// DELETE from emails where id=10 OPTION (OPTIMIZE FOR UNKNOWN);
```

## Batch Delete

Hapus seluruh catatan yang cocok

```go
db.Where("email LIKE ?", "%jinzhu%").Delete(Email{})
//// DELETE from emails where email LIKE "%jinzhu%";

db.Delete(Email{}, "email LIKE ?", "%jinzhu%")
//// DELETE from emails where email LIKE "%jinzhu%";
```

## Soft Delete

If model has `DeletedAt` field, it will get soft delete ability automatically! then it won't be deleted from database permanently when call `Delete`, but only set field `DeletedAt`'s value to current time

```go
db.Delete(&user)
//// UPDATE users SET deleted_at="2013-10-29 10:23" WHERE id = 111;

// Batch Delete
db.Where("age = ?", 20).Delete(&User{})
//// UPDATE users SET deleted_at="2013-10-29 10:23" WHERE age = 20;

// Soft deleted records will be ignored when query them
db.Where("age = 20").Find(&user)
//// SELECT * FROM users WHERE age = 20 AND deleted_at IS NULL;

// Find soft deleted records with Unscoped
db.Unscoped().Where("age = 20").Find(&users)
//// SELECT * FROM users WHERE age = 20;

// Delete record permanently with Unscoped
db.Unscoped().Delete(&order)
//// DELETE FROM orders WHERE id=10;
```