# Responsive UI – Flutter

## Source Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    double width = MediaQuery.of(context).size.width;

    return MaterialApp(
      debugShowCheckedModeBanner: true,
      home: Scaffold(
        appBar: AppBar(
          title: Text("Responsive UI"),
          actions: width >= 600
              ? [
                  TextButton(
                    onPressed: () {},
                    child: Text(
                      "Home",
                      style: TextStyle(color: Colors.black),
                    ),
                  ),
                  TextButton(
                    onPressed: () {},
                    child: Text(
                      "About",
                      style: TextStyle(color: Colors.black),
                    ),
                  ),
                  TextButton(
                    onPressed: () {},
                    child: Text(
                      "Service",
                      style: TextStyle(color: Colors.black),
                    ),
                  ),
                  TextButton(
                    onPressed: () {},
                    child: Text(
                      "Contact",
                      style: TextStyle(color: Colors.black),
                    ),
                  ),
                ]
              : [
                  PopupMenuButton<String>(
                    icon: Icon(
                      Icons.more_vert,
                      color: Colors.black,
                    ),
                    onSelected: (value) {
                      if (value == "Home") {
                        print("Home clicked");
                      } else if (value == "About") {
                        print("About clicked");
                      } else if (value == "Service") {
                        print("Service clicked");
                      } else if (value == "Contact") {
                        print("Contact clicked");
                      }
                    },
                    itemBuilder: (BuildContext context) {
                      return [
                        PopupMenuItem(
                          value: "Home",
                          child: Text("Home"),
                        ),
                        PopupMenuItem(
                          value: "About",
                          child: Text("About"),
                        ),
                        PopupMenuItem(
                          value: "Service",
                          child: Text("Service"),
                        ),
                        PopupMenuItem(
                          value: "Contact",
                          child: Text("Contact"),
                        ),
                      ];
                    },
                  ),
                ],
        ),
        body: Container(
          width: double.infinity,
          height: double.infinity,
          child: Stack(
            fit: StackFit.expand,
            children: [
              Image.network(
                "https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?w=1200",
                fit: BoxFit.cover,
                errorBuilder: (context, error, stackTrace) {
                  return Container(
                    color: Colors.blueGrey,
                  );
                },
              ),
              Center(
                child: width < 600
                    ? Text(
                        "Mobile Screen",
                        style: TextStyle(
                          fontSize: 25,
                          color: Colors.white,
                        ),
                      )
                    : Text(
                        "Large Screen",
                        style: TextStyle(
                          fontSize: 25,
                          color: Colors.white,
                        ),
                      ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

## OUTPUT

<img width="792" height="571" alt="image" src="https://github.com/user-attachments/assets/5f4448ca-6e6c-46df-b249-247f29f38eb3" />

<img width="520" height="578" alt="image" src="https://github.com/user-attachments/assets/f7858bc0-aeb0-4369-885b-9739d94d047e" />



