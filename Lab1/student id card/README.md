# Student ID Card – Flutter

## Source Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    MaterialApp(
      home: Scaffold(
        backgroundColor: const Color(0xffece3e2),
        appBar: AppBar(
          title: const Text('Student Profile Card'),
          backgroundColor: Colors.deepPurpleAccent,
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              const SizedBox(height: 30),

              CircleAvatar(
                radius: 60,
                backgroundImage: NetworkImage(
                  'https://media.licdn.com/dms/image/v2/D4D03AQGixrx0iGJCSA/profile-displayphoto-scale_200_200/B4DZuzE.MtIcAY-/0/1768235986545?e=2147483647&v=beta&t=oFiRje6uqcmdeuG6biwBRT2N0SJ1aEGjdltpjfGv_zk',
                ),
              ),

              Text(
                "Kudupudi Chaitanya Veerraju",
                style: TextStyle(
                  fontSize: 30,
                  fontWeight: FontWeight.normal,
                  fontStyle: FontStyle.normal,
                ),
              ),

              Text(
                "Roll No:23PA1A05D0",
                style: TextStyle(
                  fontSize: 20,
                  fontWeight: FontWeight.normal,
                  fontStyle: FontStyle.normal,
                ),
              ),

              Text(
                "Branch:CSE",
                style: TextStyle(
                  fontSize: 20,
                  fontWeight: FontWeight.normal,
                  fontStyle: FontStyle.normal,
                ),
              ),

              Container(
                width: 300,
                height: 30,
                color: Color(0xffdbd5d5),
                child: Row(
                  children: [
                    Icon(
                      Icons.phone_callback_outlined,
                      color: Colors.grey,
                      size: 20,
                    ),
                    Text(
                      "91 9848819777",
                      style: TextStyle(
                        fontSize: 20,
                        fontWeight: FontWeight.normal,
                        fontStyle: FontStyle.normal,
                      ),
                    ),
                  ],
                ),
              ),

              SizedBox(height: 8),

              Container(
                width: 300,
                height: 30,
                color: Color(0xffdbd5d5),
                child: Row(
                  children: [
                    Icon(
                      Icons.email,
                      color: Colors.grey,
                      size: 20,
                    ),
                    Text(
                      "23pa1a05d0@vishnu.edu.in",
                      style: TextStyle(
                        fontSize: 20,
                        fontWeight: FontWeight.normal,
                        fontStyle: FontStyle.normal,
                      ),
                    ),
                  ],
                ),
              ),
            ],
          ),
        ),
      ),
    ),
  );
}
```

## OUTPUT 

<img width="749" height="574" alt="image" src="https://github.com/user-attachments/assets/bf0c4483-7e42-4361-83b9-9d2140fceebe" />
