
# Ex.No:1 To create a employee details fields and to display the employee details using Firebase Database in Android Studio.


## AIM:

To create and display the employee details using Firebase Database in Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min.required Artic Fox)

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as HelloWorld and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Display the employee details in MainActivity file.

Step 7: Save and run the application.

## PROGRAM:
```
/*
Program to print the DatabaseTable using the firebasedatabase”.
Developed by:
Registeration Number :
*/
```
## MainActivity.java
```python
package com.example.exp1;


import android.os.Bundle;
import android.widget.*;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import com.google.firebase.database.*;
import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {

    EditText etName, etAge, etSalary;
    Button btnAdd, btnUpdate, btnDelete;
    ListView listView;

    DatabaseReference dbRef;

    ArrayList<String> items = new ArrayList<>();
    ArrayList<String> keys = new ArrayList<>();

    ArrayAdapter<String> adapter;
    String selectedKey = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etName = findViewById(R.id.etName);
        etAge = findViewById(R.id.etAge);
        etSalary = findViewById(R.id.etSalary);

        btnAdd = findViewById(R.id.btnAdd);
        btnUpdate = findViewById(R.id.btnUpdate);
        btnDelete = findViewById(R.id.btnDelete);
        listView = findViewById(R.id.listView);

        dbRef = FirebaseDatabase.getInstance()
                .getReference("Employees");

        adapter = new ArrayAdapter<>(
                this,
                android.R.layout.simple_list_item_1,
                items
        );

        listView.setAdapter(adapter);

        dbRef.addValueEventListener(new ValueEventListener() {
            @Override
            public void onDataChange(@NonNull DataSnapshot snapshot) {
                items.clear();
                keys.clear();

                for (DataSnapshot d : snapshot.getChildren()) {
                    Emp e = d.getValue(Emp.class);

                    if (e != null) {
                        items.add(e.name + " | "
                                + e.age + " | " + e.salary);
                        keys.add(d.getKey());
                    }
                }

                adapter.notifyDataSetChanged();
            }

            @Override
            public void onCancelled(@NonNull DatabaseError error) {
                Toast.makeText(MainActivity.this,
                        "Failed to load data",
                        Toast.LENGTH_SHORT).show();
            }
        });

        btnAdd.setOnClickListener(v -> {
            Emp e = new Emp(
                    etName.getText().toString(),
                    etAge.getText().toString(),
                    etSalary.getText().toString()
            );

            dbRef.push().setValue(e);
            clearFields();
        });

        btnUpdate.setOnClickListener(v -> {
            if (selectedKey != null) {
                Emp e = new Emp(
                        etName.getText().toString(),
                        etAge.getText().toString(),
                        etSalary.getText().toString()
                );

                dbRef.child(selectedKey).setValue(e);
                clearFields();
            } else {
                Toast.makeText(this,
                        "Select an item first",
                        Toast.LENGTH_SHORT).show();
            }
        });

        btnDelete.setOnClickListener(v -> {
            if (selectedKey != null) {
                dbRef.child(selectedKey).removeValue();
                clearFields();
            } else {
                Toast.makeText(this,
                        "Select an item first",
                        Toast.LENGTH_SHORT).show();
            }
        });

        listView.setOnItemClickListener((p, v, pos, id) -> {
            selectedKey = keys.get(pos);

            dbRef.child(selectedKey).get()
                    .addOnSuccessListener(d -> {
                        Emp e = d.getValue(Emp.class);

                        if (e != null) {
                            etName.setText(e.name);
                            etAge.setText(e.age);
                            etSalary.setText(e.salary);
                        }
                    });
        });
    }

    private void clearFields() {
        etName.setText("");
        etAge.setText("");
        etSalary.setText("");
        selectedKey = null;
    }
}

```
## activity_main.xml
```python
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="#F0F4FF"
    android:padding="16dp"
    tools:context=".MainActivity">

    <!-- Header -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="81dp"
        android:background="#5D5D7A"
        android:elevation="4dp"
        android:gravity="center"
        android:orientation="vertical"
        android:padding="24dp">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:fontFamily="sans-serif-smallcaps"
            android:gravity="center"
            android:text="Employee Management"
            android:textColor="#FFFFFF"
            android:textSize="24sp"
            android:textStyle="bold" />

    </LinearLayout>

    <!-- Input Form Card -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="205dp"
        android:layout_marginTop="16dp"
        android:background="#FFFFFF"
        android:elevation="4dp"
        android:orientation="vertical"
        android:padding="16dp">

        <!-- Name -->
        <EditText
            android:id="@+id/etName"
            android:layout_width="match_parent"
            android:layout_height="44dp"
            android:backgroundTint="#7655F6"
            android:hint="Enter Name"
            android:inputType="textPersonName"
            android:padding="12dp"
            android:singleLine="true"
            android:textColor="#172554"
            android:textColorHint="#64748B" />

        <!-- Age and Salary Row -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="12dp"
            android:orientation="horizontal">

            <EditText
                android:id="@+id/etAge"
                android:layout_width="0dp"
                android:layout_height="match_parent"
                android:layout_marginEnd="8dp"
                android:layout_weight="1"
                android:backgroundTint="#22C55E"
                android:hint="Age"
                android:inputType="number"
                android:padding="12dp"
                android:singleLine="true"
                android:textColor="#172554"
                android:textColorHint="#64748B" />

            <EditText
                android:id="@+id/etSalary"
                android:layout_width="0dp"
                android:layout_height="match_parent"
                android:layout_weight="2"
                android:backgroundTint="#F59E0B"
                android:hint="Salary"
                android:inputType="numberDecimal"
                android:padding="12dp"
                android:singleLine="true"
                android:textColor="#172554"
                android:textColorHint="#64748B" />

        </LinearLayout>

        <!-- Action Buttons -->
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="18dp"
            android:gravity="center"
            android:orientation="horizontal">

            <Button
                android:id="@+id/btnAdd"
                android:layout_width="0dp"
                android:layout_height="50dp"
                android:layout_marginEnd="5dp"
                android:layout_weight="1"
                android:backgroundTint="#16A34A"
                android:padding="4dp"
                android:text="Add"
                android:textAllCaps="false"
                android:textColor="#FFFFFF"
                android:textSize="14sp" />

            <Button
                android:id="@+id/btnUpdate"
                android:layout_width="0dp"
                android:layout_height="50dp"
                android:layout_marginHorizontal="5dp"
                android:layout_weight="1"
                android:backgroundTint="#2563EB"
                android:padding="4dp"
                android:text="Update"
                android:textAllCaps="false"
                android:textColor="#FFFFFF"
                android:textSize="14sp" />

            <Button
                android:id="@+id/btnDelete"
                android:layout_width="0dp"
                android:layout_height="50dp"
                android:layout_marginStart="5dp"
                android:layout_weight="1"
                android:backgroundTint="#E11D48"
                android:padding="4dp"
                android:text="Delete"
                android:textAllCaps="false"
                android:textColor="#FFFFFF"
                android:textSize="14sp" />

        </LinearLayout>

    </LinearLayout>

    <!-- Employee List Heading -->
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Employee List"
        android:textSize="21sp"
        android:textStyle="bold"
        android:textColor="#172554"
        android:layout_marginTop="20dp"
        android:layout_marginBottom="10dp"
        android:padding="4dp" />

    <!-- Employee List -->
    <ListView
        android:id="@+id/listView"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:background="#FFFFFF"
        android:divider="#E2E8F0"
        android:dividerHeight="1dp"
        android:padding="8dp"
        android:clipToPadding="false" />

/LinearLayout>
```
## Emp.java
```python
package com.example.exp1;
public class Emp {

    public String name;
    public String age;
    public String salary;
    public Emp() {
        // Required by Firebase
    }
    public Emp(String name, String age, String salary) {
        this.name = name;
        this.age = age;
        this.salary = salary;
    }
}
```
## OUTPUT
<img width="1333" height="713" alt="Screenshot 2026-11-02 091526" src="https://github.com/user-attachments/assets/05f60fb5-64d5-4bdf-a750-3661ac62e4d6" />

<img width="327" height="644" alt="image" src="https://github.com/user-attachments/assets/392f7e40-fa95-44f2-9f31-477ffc0175a4" />
<img width="328" height="645" alt="image" src="https://github.com/user-attachments/assets/1824c294-f7eb-4fe1-872f-4a6abe132507" />
<img width="1364" height="602" alt="image" src="https://github.com/user-attachments/assets/53b1db78-c635-4835-a0b4-80b77a167e78" />




## RESULT
Thus a Simple Android Application create a firebase database and to display the employee details using Firbase Real Time Database in Android Studio is developed and executed successfully.
