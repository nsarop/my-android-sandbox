```xml


<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="@color/primary"
    tools:context=".companydetailsActivity">


    <TextView
        android:id="@+id/textViewcname"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="company name"
        android:textSize="20dp"
        android:textColor="@color/icons"
        android:layout_weight="1"/>

    <TextView
        android:id="@+id/textViewcregion"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="company region"
        android:textSize="20dp"
        android:textColor="@color/icons"
        android:layout_weight="1"/>

    <TextView
        android:id="@+id/textViewccategory"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="company category"
        android:textSize="20dp"
        android:textColor="@color/icons"
        android:layout_weight="1"/>

    <TextView
        android:id="@+id/textViewpaddress"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Physical address"
        android:textSize="20dp"
        android:textColor="@color/icons"
        android:layout_weight="1"/>

    <TextView
        android:id="@+id/textViewcbio"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="company biography"
        android:textSize="20dp"
        android:textColor="@color/icons"
        android:layout_weight="4"/>

    <Button
        android:id="@+id/buttoncphone"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="phone"
        android:textAllCaps="false"
        android:textStyle="bold"
        android:textSize="25sp"
        android:drawableRight="@drawable/mobile"
        android:background="@drawable/shapes"
        />

    <Button
        android:id="@+id/buttoncemail"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Email"
        android:textAllCaps="false"
        android:textStyle="bold"
        android:textSize="25sp"
        android:drawableRight="@drawable/mobile"
        android:background="@drawable/shapes"
        />
</LinearLayout>
