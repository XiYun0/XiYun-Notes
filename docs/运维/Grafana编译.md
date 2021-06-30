# 编译

Mac编译教程：https://www.cnblogs.com/laolieren/p/build_grafana_on_mac.html

## 后端

```
go run build.go setup
go run build.go build 
```



```
bin/grafana-server -homepath /Users/glong/project/grafana
```

## 前端

```
# 如果没有安装yarn
sudo npm install -g yarn


yarn start
```

> 如果有127错误，参考https://zhuanlan.zhihu.com/p/331874983



## 汉化

教程：https://www.bilibili.com/read/cv6735751/

修改服务器上的/usr/share/grafana/public/app

```
修改登录页

app/core/components/Login/LoginForm.tsx

修改Change Password

app/features/profile/ChangePasswordPage.tsx 

app/features/profile/ChangePasswordForm.tsx 


修改偏好设置Preferences

app/features/org/SelectOrgCtrl.ts

app/core/components/SharedPreferences/SharedPreferences.tsx

app/features/admin/partials/edit_user.html

app/features/profile/ChangePasswordForm.tsx
```



可以直接参考！：https://blog.csdn.net/qq_29860591/article/details/108634571



