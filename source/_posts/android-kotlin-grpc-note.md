---
title: Android Kotlin使用gRPC
date: 2022-03-24 19:00:00
tags: 
  - android
  - kotlin
  - grpc
  - protobuf
categories:
  - 技术
  - Android
---

# Android Kotlin使用gRPC记录

## 修改Project gradle kts

### 添加plugin

```groovy
    id('com.google.protobuf') version '0.8.18'
```

### 添加dependencies

```groovy
    implementation('io.grpc:grpc-protobuf:1.45.0')
    // gRPC请求依赖网络库，需指定netty或okhttp
    implementation('io.grpc:grpc-okhttp:1.45.0')
    implementation("io.grpc:grpc-stub:1.45.0")
    implementation("io.grpc:grpc-kotlin-stub:1.2.1")

    implementation("com.google.protobuf:protobuf-kotlin:3.19.4")
```

### 添加protobuf block

```groovy
// The standard protobuf block, same as normal gRPC Java projects
protobuf {
    // The artifact spec for the Protobuf Compiler
    protoc { artifact = 'com.google.protobuf:protoc:3.19.4' }
    // Optional: an artifact spec for a protoc plugin, with "grpc" as
    // the identifier, which can be referred to in the "plugins"
    // container of the "generateProtoTasks" closure.
    plugins {
        grpc { artifact = "io.grpc:protoc-gen-grpc-java:1.45.0" }
        grpckt { artifact = "io.grpc:protoc-gen-grpc-kotlin:1.2.1:jdk7@jar" }
    }
    generateProtoTasks {
        all()*.plugins {
          	// 没有这句，不生成 message.java
            java {}
          	// 没有这句，不生成 message.kt
            kotlin {}
          	// 没有这句，不生成 grpc.java
            grpc {}
          	// 没有这句，不生成 grpc.kt
            grpckt {}
        }
    }
}
```

实际上kotlin是生成java代码的包装，kotlin依赖生成的java代码，无法直接生成kotlin使用（也许我不知道有办法？）

### 添加网络权限

```xml
    <uses-permission android:name="android.permission.INTERNET" />
```

否则请求抛异常 StatusRuntimeException UNAVAILABLE
