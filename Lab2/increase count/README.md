
# Stateful Widget Example – Flutter

## Source Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    MyApp(),
  );
}

class MyApp extends StatefulWidget {
  @override
  State<MyApp> createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  int c = 0;

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
            appBar: AppBar(
              title: Text(
                "Stateful Widget Example",
                style: TextStyle(
                  fontWeight: FontWeight.w400,
                ),
              ),
            ),
            body: Center(
                child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Icon(
                  Icons.school,
                  color: Colors.red,
                  size: 50,
                ),
                SizedBox(height: 10),
                Text(
                  "Count:$c",
                  style: TextStyle(fontSize: 20),
                ),
                SizedBox(height: 20),
                ElevatedButton(
                  onPressed: () {
                    setState(() {
                      c++;
                    });
                  },
                  child: Text("Click Me"),
                ),
              ],
            ))));
  }
}
```

## OUTPUT

<img width="335" height="566" alt="image" src="https://github.com/user-attachments/assets/aa776cf1-9fe4-4df6-bea2-9c8f3be83afe" />

