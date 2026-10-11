# Menu List App – Flutter

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
      title: 'Chaitanya Menu App',
      home: MenuScreen(),
    );
  }
}

// Reusable Internet Background
Widget backgroundImage(Widget child) {
  return Container(
    width: double.infinity,
    height: double.infinity,
    decoration: BoxDecoration(
      image: DecorationImage(
        image: NetworkImage(
          "https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?w=1200",
        ),
        fit: BoxFit.cover,
        onError: (exception, stackTrace) {},
      ),
    ),
    child: Container(
      color: Colors.black.withOpacity(0.35),
      child: child,
    ),
  );
}

// Menu Screen
class MenuScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Menu List"),
        backgroundColor: Colors.blue,
        foregroundColor: Colors.white,
      ),
      body: backgroundImage(
        ListView(
          padding: EdgeInsets.all(20),
          children: [
            SizedBox(height: 30),
            Text(
              "Welcome, Chaitanya!",
              textAlign: TextAlign.center,
              style: TextStyle(
                fontSize: 30,
                fontWeight: FontWeight.bold,
                color: Colors.white,
              ),
            ),
            SizedBox(height: 30),
            Card(
              child: ListTile(
                leading: Icon(Icons.home),
                title: Text("Home"),
                trailing: Icon(Icons.arrow_forward_ios),
                onTap: () {
                  Navigator.push(
                    context,
                    MaterialPageRoute(
                      builder: (context) => HomePage(),
                    ),
                  );
                },
              ),
            ),
            Card(
              child: ListTile(
                leading: Icon(Icons.build),
                title: Text("Services"),
                trailing: Icon(Icons.arrow_forward_ios),
                onTap: () {
                  Navigator.push(
                    context,
                    MaterialPageRoute(
                      builder: (context) => ServicesPage(),
                    ),
                  );
                },
              ),
            ),
            Card(
              child: ListTile(
                leading: Icon(Icons.feedback),
                title: Text("Feedback"),
                trailing: Icon(Icons.arrow_forward_ios),
                onTap: () {
                  Navigator.push(
                    context,
                    MaterialPageRoute(
                      builder: (context) => FeedbackPage(),
                    ),
                  );
                },
              ),
            ),
          ],
        ),
      ),
    );
  }
}

// Home Page
class HomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Home"),
        backgroundColor: Colors.blue,
        foregroundColor: Colors.white,
      ),
      body: backgroundImage(
        Center(
          child: Padding(
            padding: EdgeInsets.all(20),
            child: Text(
              "Welcome, Chaitanya Kudupudi!\n\n"
              "Welcome to the Home Page",
              textAlign: TextAlign.center,
              style: TextStyle(
                fontSize: 25,
                color: Colors.white,
                fontWeight: FontWeight.bold,
              ),
            ),
          ),
        ),
      ),
    );
  }
}

// Services Page
class ServicesPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Services"),
        backgroundColor: Colors.blue,
        foregroundColor: Colors.white,
      ),
      body: backgroundImage(
        Center(
          child: Text(
            "Our Services\n\n"
            "Web Development\n"
            "Mobile App Development\n"
            "UI/UX Design\n"
            "Software Development",
            textAlign: TextAlign.center,
            style: TextStyle(
              fontSize: 22,
              color: Colors.white,
              height: 1.8,
              fontWeight: FontWeight.bold,
            ),
          ),
        ),
      ),
    );
  }
}

// Feedback Page
class FeedbackPage extends StatefulWidget {
  @override
  State<FeedbackPage> createState() => _FeedbackPageState();
}

class _FeedbackPageState extends State<FeedbackPage> {
  final TextEditingController feedbackController =
      TextEditingController();

  String message = "";

  void submitFeedback() {
    if (feedbackController.text.trim().isNotEmpty) {
      setState(() {
        message = "Feedback Submitted Successfully!";
      });
    } else {
      setState(() {
        message = "Please enter your feedback.";
      });
    }
  }

  @override
  void dispose() {
    feedbackController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Feedback"),
        backgroundColor: Colors.blue,
        foregroundColor: Colors.white,
      ),
      body: backgroundImage(
        Padding(
          padding: EdgeInsets.all(20),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Text(
                "Enter Your Feedback",
                style: TextStyle(
                  fontSize: 25,
                  color: Colors.white,
                  fontWeight: FontWeight.bold,
                ),
              ),
              SizedBox(height: 20),
              TextField(
                controller: feedbackController,
                maxLines: 4,
                decoration: InputDecoration(
                  hintText: "Enter your feedback",
                  filled: true,
                  fillColor: Colors.white,
                  border: OutlineInputBorder(),
                ),
              ),
              SizedBox(height: 20),
              ElevatedButton(
                onPressed: submitFeedback,
                child: Text("Submit"),
              ),
              SizedBox(height: 20),
              Text(
                message,
                textAlign: TextAlign.center,
                style: TextStyle(
                  fontSize: 20,
                  color: Colors.white,
                  fontWeight: FontWeight.bold,
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

<img width="894" height="588" alt="image" src="https://github.com/user-attachments/assets/ae478cfe-3e51-4688-b825-63d5d6e2d883" />


<img width="877" height="570" alt="image" src="https://github.com/user-attachments/assets/de812922-b0c4-4cdd-8fa7-0a89815f65f4" />

<img width="873" height="563" alt="image" src="https://github.com/user-attachments/assets/b39a1fed-c5ee-4c7f-aa86-ea3d94fd3736" />

<img width="888" height="578" alt="image" src="https://github.com/user-attachments/assets/0d280789-8db7-46d7-b2b6-e0c929b93222" />
