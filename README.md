# Ex.No:2 Develop an android application to implement the AIDL server and client app. The server app hosts a Bound Service and contains the logic to return random colours to its client.The client app calls the service and changes the button's colour within the Main activity.



## AIM:

To Develop an android application to implement the AIDL server and client app. The server app hosts a Bound Service and contains the logic to return random colours to its client.
The client app calls the service and changes the button's colour within the Main activity using AIDL interface in Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min.required Griaffe )

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as CSAIDL and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Display message give in MainActivity file(client/server).

Step 7: Save and run the application.

## PROGRAM:
```
/*
Program to print the client/server services using AIDL”.
Developed by: Ramkumar S
Registeration Number : 212223220085
*/
```
## AIDL CLIENT APP
Main acticity.java
```
package com.example.aidlclient;

import android.content.ComponentName;
import android.content.Context;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.Bundle;
import android.os.IBinder;
import android.os.RemoteException;
import android.widget.Button;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

import com.example.aidlserver.IColorService; // Copied AIDL interface

public class MainActivity extends AppCompatActivity {

    private IColorService colorService;
    private boolean isBound = false;
    private Button btnChangeColor;

    private final ServiceConnection connection = new ServiceConnection() {

        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            colorService = IColorService.Stub.asInterface(service);
            isBound = true;

            Toast.makeText(
                    MainActivity.this,
                    "Connected",
                    Toast.LENGTH_SHORT
            ).show();
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
            isBound = false;
            colorService = null;
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        setContentView(R.layout.activity_main);

        btnChangeColor = findViewById(R.id.btnChangeColor);

        btnChangeColor.setOnClickListener(v -> {

            if (isBound && colorService != null) {

                try {
                    int randomColor = colorService.getRandomColor();

                    btnChangeColor.setBackgroundColor(randomColor);

                } catch (RemoteException e) {
                    Toast.makeText(
                            this,
                            "Error",
                            Toast.LENGTH_SHORT
                    ).show();
                }

            } else {
                Toast.makeText(
                        this,
                        "Not connected",
                        Toast.LENGTH_SHORT
                ).show();
            }
        });
    }

    @Override
    protected void onStart() {
        super.onStart();

        // Bind to the server's service
        Intent intent = new Intent(
                "com.example.aidlserver.IColorService"
        );

        // Tells Android which app contains the service
        intent.setPackage("com.example.aidlserver");

        bindService(
                intent,
                connection,
                Context.BIND_AUTO_CREATE
        );
    }

    @Override
    protected void onStop() {
        super.onStop();

        if (isBound) {
            unbindService(connection);
            isBound = false;
        }
    }
}
```
activity_main.xml
```
<?xml version="1.0" encoding="utf-8"?>

<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical">

    <Button
        android:id="@+id/btnChangeColor"
        android:layout_width="200dp"
        android:layout_height="200dp"
        android:text="Tap me!"
        android:textSize="24sp" />

</LinearLayout>
```
AndroidManifest.xml
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <queries>
        <package android:name="com.example.aidlserver" />
    </queries>

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Aidlclient">
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:windowSoftInputMode="adjustResize">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

## OUTPUT

## AIDL SERVER

<img width="1600" height="848" alt="WhatsApp Image 2026-08-21 at 10 29 42 PM" src="https://github.com/user-attachments/assets/b67e8cb5-bb40-4e98-919b-e5b1177a814d" />


## AIDL CLIENT

<img width="1600" height="848" alt="WhatsApp Image 2026-08-21 at 10 30 10 PM" src="https://github.com/user-attachments/assets/5a7c906a-61c6-4171-a278-e8dbecf77bc4" />

<img width="1600" height="848" alt="WhatsApp Image 2026-08-21 at 10 30 43 PM" src="https://github.com/user-attachments/assets/5b11a87d-1b16-4ef7-b03e-05797025655d" />


## RESULT
Thus a Simple Android Application to create a AIDL interface and communicate the process between client and server using AIDL interface in Android Studio is developed and executed successfully.
