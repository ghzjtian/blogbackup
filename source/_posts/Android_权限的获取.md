---
title: Android_权限的获取
tags: [Android]
categories: [Android]
date: 2017-12-19 17:46:19
---



### [Android_权限的获取](https://github.com/ghzjtian/AndroidPermission)

* 1.Android 6.0 后，敏感的权限需要用户实时的同意才可以获取.第三方库 PermissionsDispatcher 的用法.

* [2.Android 官方 系统权限 的解析](https://developer.android.com/guide/topics/security/permissions?hl=zh-cn#normal-dangerous)

<!-- more -->

* 参考:

>https://github.com/permissions-dispatcher/PermissionsDispatcher
>
>http://www.jianshu.com/p/64e7334cde11



#### 1.用法（ Camera 权限）:

* 1.在 app module 的 build.gradle 中，加入以下的代码:

```Android
    compile("com.github.hotchemi:permissionsdispatcher:3.1.0") {
        // if you don't use android.app.Fragment you can exclude support for them
        exclude module: "support-v13"
    }
    annotationProcessor "com.github.hotchemi:permissionsdispatcher-processor:3.1.0"


```

在 `AndroidManifest.xml` 中，添加以下的权限
```
<uses-permission android:name="android.permission.CAMERA"/>
```

* 2.在需要获取权限的 Activity 下，添加如下的代码:

```Android

package com.example.tianzeng.testpermissions;

import android.Manifest;
import android.content.DialogInterface;
import android.os.Bundle;
import android.support.annotation.NonNull;
import android.support.annotation.StringRes;
import android.support.design.widget.FloatingActionButton;
import android.support.design.widget.Snackbar;
import android.support.v7.app.AlertDialog;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.view.View;
import android.widget.Toast;

import permissions.dispatcher.NeedsPermission;
import permissions.dispatcher.OnNeverAskAgain;
import permissions.dispatcher.OnPermissionDenied;
import permissions.dispatcher.OnShowRationale;
import permissions.dispatcher.PermissionRequest;
import permissions.dispatcher.RuntimePermissions;

@RuntimePermissions
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        findViewById(R.id.button_camera).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
//                MainActivityPermissionsDispatcher.showCameraWithPermissionCheck(MainActivity.this);
            }
        });


    }




    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        // NOTE: delegate the permission handling to generated method
//        MainActivityPermissionsDispatcher.onRequestPermissionsResult(this, requestCode, grantResults);
    }

    @NeedsPermission(Manifest.permission.CAMERA)
    void showCamera() {
        // NOTE: Perform action that requires the permission. If this is run by PermissionsDispatcher, the permission will have been granted
        getSupportFragmentManager().beginTransaction()
                .replace(R.id.sample_content_fragment, CameraPreviewFragment.newInstance())
                .addToBackStack("camera")
                .commitAllowingStateLoss();
    }

    @OnPermissionDenied(Manifest.permission.CAMERA)
    void onCameraDenied() {
        // NOTE: Deal with a denied permission, e.g. by showing specific UI
        // or disabling certain functionality
        Toast.makeText(this, R.string.permission_camera_denied, Toast.LENGTH_SHORT).show();
    }

    @OnShowRationale(Manifest.permission.CAMERA)
    void showRationaleForCamera(PermissionRequest request) {
        // NOTE: Show a rationale to explain why the permission is needed, e.g. with a dialog.
        // Call proceed() or cancel() on the provided PermissionRequest to continue or abort
        showRationaleDialog(R.string.permission_camera_rationale, request);
    }

    @OnNeverAskAgain(Manifest.permission.CAMERA)
    void onCameraNeverAskAgain() {
        Toast.makeText(this, R.string.permission_camera_never_ask_again, Toast.LENGTH_SHORT).show();
    }

    private void showRationaleDialog(@StringRes int messageResId, final PermissionRequest request) {
        new AlertDialog.Builder(this)
                .setPositiveButton(R.string.button_allow, new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(@NonNull DialogInterface dialog, int which) {
                        request.proceed();
                    }
                })
                .setNegativeButton(R.string.button_deny, new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(@NonNull DialogInterface dialog, int which) {
                        request.cancel();
                    }
                })
                .setCancelable(false)
                .setMessage(messageResId)
                .show();
    }

}

```

* 3.在 Build -> Make Module **** , PermissionsDispatcher 会生成 ainActivityPermissionsDispatcher(Activity Name + PermissionsDispatcher).


* 4.打开步骤 2 的注释的代码:

```Android
MainActivityPermissionsDispatcher.showCameraWithPermissionCheck(MainActivity.this);
       MainActivityPermissionsDispatcher.onRequestPermissionsResult(this, requestCode, grantResults);
 

```




