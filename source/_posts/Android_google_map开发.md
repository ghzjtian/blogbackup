---
title: Android Google Map 开发
tags: [Android]
categories: [Android]
date: 2018-09-11 17:46:19
---

> 记录 Android_google开发 相关的过程

<!-- more -->


## [1.参考](#references)
## [2.Google 例子](#google_map_demo)
## [3.取得当前位置的经纬度](#get_current_location)
## [4.国内国外地图切换](#amap_switch)

***
***
***

## 1.参考<a name="references"/>
* [1.Maps SDK for Android:Get Started](https://developers.google.com/maps/documentation/android-sdk/start)
* [2.GitHub:Google Maps Android API v2 Samples](https://github.com/googlemaps/android-samples)
* [3.高德:国内国外地图切换](https://lbs.amap.com/dev/demo/switch-map#Android)


***

## 2.Google 例子<a name="google_map_demo"/>
> 记录 Google maps Demo 的搭建过程.

* 1.从 [GitHub google maps 下载 DEMO](https://github.com/googlemaps/android-samples)
* 2.用 AS 打开 `DEMO` 中的 `ApiDemos/java` 项目(因为我们只要开发 java 环境下的 手机端 map)
* 3.到 [Google Maps 控制台](https://console.developers.google.com/) 去申请一个 `API KEY`,然后填入项目 `AndroidManifest.xml` 中.
	* 1.过程中需要用到 `DEBUG` 和 `RELEASE` 的 `SHA-1`,具体的方法请查看: [Get API Key](https://developers.google.com/maps/documentation/android-sdk/signup)
	* 2.相关的图片:
<img src="/assets/imgs/Android/Snip20180911_11.png" width="80%" height="80%">
	* 3.把得到的 KEY 填入 `AndroidManifest.xml` 的 `android:name="com.google.android.geo.API_KEY` 中.....

***

## 3.取得当前位置的经纬度<a name="get_current_location"/>

#### 1.相关的包的导入
>不要在 `build.gradle` 中直接导入 `compile 'com.google.android.gms:play-services:9.0.1'` ,因为 Android 的项目有 `65535` 的限制,要根据需要导入必要的包: 如:

```
compile 'com.google.android.gms:play-services-maps:9.0.1'
compile 'com.google.android.gms:play-services-location:9.0.1'
```

* [1.参考1](https://stackoverflow.com/questions/25370598/cant-import-com-google-android-gms-location-locationservices)
* [2.参考2](https://developers.google.com/android/guides/setup#add_google_play_services_to_your_project)

#### 2.获取当前位置的代码,[相关的 GitHub 源码](https://github.com/ghzjtian/android-samples):

```
package com.example.mapdemo;


import android.content.pm.PackageManager;
import android.location.Location;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.support.v4.app.ActivityCompat;
import android.support.v4.app.FragmentActivity;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.util.Log;
import android.view.View;
import android.widget.Toast;

import com.google.android.gms.common.ConnectionResult;
import com.google.android.gms.common.api.GoogleApiClient;
import com.google.android.gms.location.FusedLocationProviderClient;
import com.google.android.gms.location.LocationServices;
import com.google.android.gms.maps.CameraUpdateFactory;
import com.google.android.gms.maps.GoogleMap;
import com.google.android.gms.maps.OnMapReadyCallback;
import com.google.android.gms.maps.SupportMapFragment;
import com.google.android.gms.maps.model.LatLng;
import com.google.android.gms.maps.model.Marker;
import com.google.android.gms.maps.model.MarkerOptions;
import com.google.android.gms.tasks.OnSuccessListener;

public class GetMyLocationDemoActivity extends AppCompatActivity implements OnMapReadyCallback,
        GoogleApiClient.ConnectionCallbacks,
        GoogleApiClient.OnConnectionFailedListener,
        GoogleMap.OnMarkerDragListener,
        GoogleMap.OnMapLongClickListener,
        GoogleMap.OnMarkerClickListener,
        View.OnClickListener {

    private static final String TAG = "MapsActivity";
    private GoogleMap mMap;
    private double longitude;
    private double latitude;
    private GoogleApiClient googleApiClient;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.my_location_demo);

        // Obtain the SupportMapFragment and get notified when the map is ready to be used.
        SupportMapFragment mapFragment = (SupportMapFragment) getSupportFragmentManager()
                .findFragmentById(R.id.map);
        mapFragment.getMapAsync(this);

        //Initializing googleApiClient
        googleApiClient = new GoogleApiClient.Builder(this)
                .addConnectionCallbacks(this)
                .addOnConnectionFailedListener(this)
                .addApi(LocationServices.API)
                .build();


    }


    @Override
    public void onMapReady(GoogleMap googleMap) {
        mMap = googleMap;
        mMap.setMapType(GoogleMap.MAP_TYPE_HYBRID);
        // googleMapOptions.mapType(googleMap.MAP_TYPE_HYBRID)
        //    .compassEnabled(true);

        // Add a marker in Sydney and move the camera
        LatLng india = new LatLng(-34, 151);
        mMap.addMarker(new MarkerOptions().position(india).title("Marker in India"));
        mMap.moveCamera(CameraUpdateFactory.newLatLng(india));
        mMap.setOnMarkerDragListener(this);
        mMap.setOnMapLongClickListener(this);
    }

    //Getting current location
    private void getCurrentLocation() {
        mMap.clear();
        if (ActivityCompat.checkSelfPermission(this, android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED && ActivityCompat.checkSelfPermission(this, android.Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {
            // TODO: Consider calling
            //    ActivityCompat#requestPermissions
            // here to request the missing permissions, and then overriding
            //   public void onRequestPermissionsResult(int requestCode, String[] permissions,
            //                                          int[] grantResults)
            // to handle the case where the user grants the permission. See the documentation
            // for ActivityCompat#requestPermissions for more details.
            return;
        }
//        Location location = LocationServices.FusedLocationApi.getLastLocation(googleApiClient);
//        if (location != null) {
//            //Getting longitude and latitude
//            longitude = location.getLongitude();
//            latitude = location.getLatitude();
//
//            //moving the map to location
//            moveMap();
//        }

        //参考:  https://stackoverflow.com/questions/46481789/android-locationservices-fusedlocationapi-deprecated
        FusedLocationProviderClient mFusedLocationClient = LocationServices.getFusedLocationProviderClient(this);

        mFusedLocationClient.getLastLocation()
                .addOnSuccessListener(this, new OnSuccessListener<Location>() {
                    @Override
                    public void onSuccess(Location location) {
                        // Got last known location. In some rare situations, this can be null.
                        if (location != null) {
                            Log.w(TAG,"location,Latitude:"+location.getLatitude()+" Longtitude:"+location.getLongitude());
                            // Logic to handle location object
                            //Getting longitude and latitude
                            longitude = location.getLongitude();
                            latitude = location.getLatitude();

                            //moving the map to location
                            moveMap();
                        }
                    }
                });

    }

    private void moveMap() {
        /**
         * Creating the latlng object to store lat, long coordinates
         * adding marker to map
         * move the camera with animation
         */
        LatLng latLng = new LatLng(latitude, longitude);
        mMap.addMarker(new MarkerOptions()
                .position(latLng)
                .draggable(true)
                .title("Marker in India"));

        mMap.moveCamera(CameraUpdateFactory.newLatLng(latLng));
        mMap.animateCamera(CameraUpdateFactory.zoomTo(15));
        mMap.getUiSettings().setZoomControlsEnabled(true);


    }

    @Override
    public void onClick(View view) {
        Log.v(TAG, "view click event");
    }

    @Override
    public void onConnected(@Nullable Bundle bundle) {
        getCurrentLocation();
    }

    @Override
    public void onConnectionSuspended(int i) {

    }

    @Override
    public void onConnectionFailed(@NonNull ConnectionResult connectionResult) {

    }

    @Override
    public void onMapLongClick(LatLng latLng) {
        // mMap.clear();
        mMap.addMarker(new MarkerOptions().position(latLng).draggable(true));
    }

    @Override
    public void onMarkerDragStart(Marker marker) {
        Toast.makeText(GetMyLocationDemoActivity.this, "onMarkerDragStart", Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onMarkerDrag(Marker marker) {
        Toast.makeText(GetMyLocationDemoActivity.this, "onMarkerDrag", Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onMarkerDragEnd(Marker marker) {
        // getting the Co-ordinates
        latitude = marker.getPosition().latitude;
        longitude = marker.getPosition().longitude;

        //move to current position
        moveMap();
    }

    @Override
    protected void onStart() {
        googleApiClient.connect();
        super.onStart();
    }

    @Override
    protected void onStop() {
        googleApiClient.disconnect();
        super.onStop();
    }


    @Override
    public boolean onMarkerClick(Marker marker) {
        Toast.makeText(GetMyLocationDemoActivity.this, "onMarkerClick", Toast.LENGTH_SHORT).show();
        return true;
    }

}
```

### 3.发现 google map 地图的坐标在国内显示有偏差
* 1.原因及解决的方法:
	* [1.地图坐标转换（火星、谷歌、百度、腾讯、高德等坐标）](https://www.jianshu.com/p/c39a2c72dc65)
* 2.转换前后的对比:
	* 1.转换前:
  <img src="/assets/imgs/Android/Screenshot_20180911-162321.png" width="50%" height="50%">
  	* 2.转换后:
  <img src="/assets/imgs/Android/Screenshot_20180911-165421.png" width="50%" height="50%">

***

## 4.国内国外地图切换<a name="amap_switch"/>
* 1.参考 [高德 国内国外地图切换](https://lbs.amap.com/dev/demo/switch-map#Android)
* [2.参考: iOS判断GPS坐标是否在中国](https://sxgfxm.github.io/blog/2016/10/19/iospan-duan-gpszuo-biao-shi-fou-zai-zhong-guo/)
* [3.高德:坐标转换与位置判断(更加的准确)](https://lbs.amap.com/api/android-location-sdk/guide/additional-func/amap-calculate-tool)





