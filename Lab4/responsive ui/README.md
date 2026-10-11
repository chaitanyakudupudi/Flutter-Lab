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
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: ResponsiveScreen(),
    );
  }
}

class ResponsiveScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return LayoutBuilder(
      builder: (context, constraints) {
        double width = constraints.maxWidth;

        return Scaffold(
          appBar: AppBar(
            title: Text("Responsive UI"),
            backgroundColor: Colors.blue,
            foregroundColor: Colors.white,
            actions: width >= 600
                ? [
                    TextButton(
                      onPressed: () => print("Home clicked"),
                      child: Text(
                        "Home",
                        style: TextStyle(color: Colors.white),
                      ),
                    ),
                    TextButton(
                      onPressed: () => print("About clicked"),
                      child: Text(
                        "About",
                        style: TextStyle(color: Colors.white),
                      ),
                    ),
                    TextButton(
                      onPressed: () => print("Service clicked"),
                      child: Text(
                        "Service",
                        style: TextStyle(color: Colors.white),
                      ),
                    ),
                    TextButton(
                      onPressed: () => print("Contact clicked"),
                      child: Text(
                        "Contact",
                        style: TextStyle(color: Colors.white),
                      ),
                    ),
                  ]
                : [
                    PopupMenuButton<String>(
                      icon: Icon(
                        Icons.more_vert,
                        color: Colors.white,
                      ),
                      onSelected: (value) {
                        print("$value clicked");
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
          body: SizedBox(
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
                      child: Center(
                        child: Text(
                          "Background image unavailable",
                          style: TextStyle(
                            color: Colors.white,
                            fontSize: 18,
                          ),
                        ),
                      ),
                    );
                  },
                ),
                Center(
                  child: Text(
                    width < 600 ? "Mobile Screen" : "Large Screen",
                    style: TextStyle(
                      fontSize: 25,
                      fontWeight: FontWeight.bold,
                      color: Colors.white,
                      shadows: [
                        Shadow(
                          color: Colors.black,
                          blurRadius: 8,
                        ),
                      ],
                    ),
                  ),
                ),
              ],
            ),
          ),
        );
      },
    );
  }
}
```

## OUTPUT

<img width="926" height="588" alt="image" src="https://github.com/user-attachments/assets/b8559eec-8773-45dd-81f0-d849a3c36c9a" />

<img width="438" height="576" alt="image" src="https://github.com/user-attachments/assets/97840fd2-dd03-4bbb-8b00-c5eda22d4e0f" />

