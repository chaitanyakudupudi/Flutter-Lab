# Student Dashboard – Flutter

## Source Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(
    const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: StudentDashboard(),
    ),
  );
}

class StudentDashboard extends StatefulWidget {
  const StudentDashboard({super.key});

  @override
  State<StudentDashboard> createState() => _StudentDashboardState();
}

class _StudentDashboardState extends State<StudentDashboard> {
  bool notification = false;
  int counter = 0;

  final String coverImage =
      "https://media.licdn.com/dms/image/v2/D561BAQF32kDtuiUB4w/company-background_10000/company-background_10000/0/1658205583234/vishnusvecw_cover?e=2147483647&v=beta&t=uOqgGeEStLZw03Vjj6wvdKa8EYs0C4SYp5lUEAsg91E";

  final String profileImage =
      "https://media.licdn.com/dms/image/v2/D4D03AQGixrx0iGJCSA/profile-displayphoto-scale_200_200/B4DZuzE.MtIcAY-/0/1768235986545?e=2147483647&v=beta&t=oFiRje6uqcmdeuG6biwBRT2N0SJ1aEGjdltpjfGv_zk";

  Widget featureBox(IconData icon, String text) {
    return Column(
      children: [
        Container(
          height: 70,
          width: 70,
          decoration: BoxDecoration(
            color: Colors.blue.shade100,
            borderRadius: BorderRadius.circular(15),
          ),
          child: Icon(icon, color: Colors.blue, size: 35),
        ),
        const SizedBox(height: 8),
        Text(
          text,
          style: const TextStyle(
            fontSize: 16,
            fontWeight: FontWeight.w500,
          ),
        ),
      ],
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Student Dashboard"),
        backgroundColor: Colors.blue,
        foregroundColor: Colors.white,
      ),
      body: SingleChildScrollView(
        child: Column(
          children: [
            Stack(
              clipBehavior: Clip.none,
              children: [
                Image.network(
                  coverImage,
                  width: double.infinity,
                  height: 220,
                  fit: BoxFit.cover,
                  webHtmlElementStrategy:
                      WebHtmlElementStrategy.prefer,
                  errorBuilder: (context, error, stackTrace) {
                    return Container(
                      width: double.infinity,
                      height: 220,
                      color: Colors.blue.shade200,
                      child: const Center(
                        child: Text("College Image"),
                      ),
                    );
                  },
                ),
                Positioned(
                  left: 20,
                  bottom: -40,
                  child: CircleAvatar(
                    radius: 45,
                    backgroundColor: Colors.grey.shade300,
                    child: ClipOval(
                      child: Image.network(
                        profileImage,
                        width: 90,
                        height: 90,
                        fit: BoxFit.cover,
                        webHtmlElementStrategy:
                            WebHtmlElementStrategy.prefer,
                        errorBuilder: (context, error, stackTrace) {
                          return const Icon(
                            Icons.person,
                            size: 55,
                            color: Colors.white,
                          );
                        },
                      ),
                    ),
                  ),
                ),
                const Positioned(
                  left: 120,
                  bottom: 25,
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(
                        "Chaitanya Kudupudi",
                        style: TextStyle(
                          color: Colors.white,
                          fontSize: 22,
                          fontWeight: FontWeight.bold,
                          shadows: [
                            Shadow(
                              color: Colors.black,
                              blurRadius: 5,
                            ),
                          ],
                        ),
                      ),
                      Text(
                        "Computer Science Student",
                        style: TextStyle(
                          color: Colors.white,
                          fontSize: 15,
                          shadows: [
                            Shadow(
                              color: Colors.black,
                              blurRadius: 5,
                            ),
                          ],
                        ),
                      ),
                    ],
                  ),
                ),
              ],
            ),
            const SizedBox(height: 60),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                featureBox(Icons.menu_book, "Books"),
                featureBox(Icons.assignment, "Tasks"),
                featureBox(Icons.school, "Marks"),
                featureBox(Icons.person, "Profile"),
              ],
            ),
            const SizedBox(height: 30),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                ElevatedButton(
                  onPressed: notification
                      ? () {
                          ScaffoldMessenger.of(context).showSnackBar(
                            const SnackBar(
                              content: Text("Login Button Pressed"),
                            ),
                          );
                        }
                      : null,
                  child: const Text("Login"),
                ),
                OutlinedButton(
                  onPressed: notification
                      ? () {
                          ScaffoldMessenger.of(context).showSnackBar(
                            const SnackBar(
                              content: Text("Register Button Pressed"),
                            ),
                          );
                        }
                      : null,
                  child: const Text("Register"),
                ),
              ],
            ),
            const SizedBox(height: 30),
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                const Text(
                  "Notifications",
                  style: TextStyle(fontSize: 20),
                ),
                Switch(
                  value: notification,
                  onChanged: (value) {
                    setState(() {
                      notification = value;
                    });
                  },
                ),
              ],
            ),
            const SizedBox(height: 20),
            const Text(
              "Counter",
              style: TextStyle(
                fontSize: 30,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 10),
            Text(
              "$counter",
              style: const TextStyle(
                fontSize: 45,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 20),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                ElevatedButton(
                  onPressed: notification
                      ? () {
                          setState(() {
                            counter++;
                          });
                        }
                      : null,
                  child: const Text("+"),
                ),
                ElevatedButton(
                  onPressed: notification
                      ? () {
                          setState(() {
                            counter--;
                          });
                        }
                      : null,
                  child: const Text("-"),
                ),
              ],
            ),
            const SizedBox(height: 30),
          ],
        ),
      ),
    );
  }
}
```

## OUTPUT

<img width="742" height="579" alt="image" src="https://github.com/user-attachments/assets/7ef06e01-6a97-4f6f-95e9-b15d26248d58" />
