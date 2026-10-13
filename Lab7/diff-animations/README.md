# Animation Effects – Flutter

## Source Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Animation Effects',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: AnimationScreen(),
    );
  }
}

class AnimationScreen extends StatefulWidget {
  AnimationScreen({super.key});

  @override
  State<AnimationScreen> createState() => _AnimationScreenState();
}

class _AnimationScreenState extends State<AnimationScreen>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;

  late Animation<double> _fadeAnimation;
  late Animation<Offset> _slideAnimation;
  late Animation<double> _scaleAnimation;
  late Animation<double> _rotationAnimation;

  @override
  void initState() {
    super.initState();

    _controller = AnimationController(
      vsync: this,
      duration: Duration(seconds: 2),
    );

    _fadeAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(
      CurvedAnimation(
        parent: _controller,
        curve: Curves.easeIn,
      ),
    );

    _slideAnimation = Tween<Offset>(
      begin: Offset(-1.0, 0.0),
      end: Offset.zero,
    ).animate(
      CurvedAnimation(
        parent: _controller,
        curve: Curves.easeOut,
      ),
    );

    _scaleAnimation = Tween<double>(
      begin: 0.2,
      end: 1.0,
    ).animate(
      CurvedAnimation(
        parent: _controller,
        curve: Curves.elasticOut,
      ),
    );

    _rotationAnimation = Tween<double>(
      begin: 0.0,
      end: 1.0,
    ).animate(
      CurvedAnimation(
        parent: _controller,
        curve: Curves.easeInOut,
      ),
    );

    _controller.forward();
  }

  void restartAnimation() {
    _controller.reset();
    _controller.forward();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  Widget animationBox(String title, IconData icon, Color color) {
    return Container(
      width: 280,
      height: 100,
      decoration: BoxDecoration(
        color: color,
        borderRadius: BorderRadius.circular(20),
      ),
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Icon(
            icon,
            color: Colors.white,
            size: 40,
          ),
          SizedBox(height: 8),
          Text(
            title,
            style: TextStyle(
              color: Colors.white,
              fontSize: 20,
              fontWeight: FontWeight.bold,
            ),
          ),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Flutter Animation Effects'),
        centerTitle: true,
        backgroundColor: Colors.blue,
        foregroundColor: Colors.white,
      ),
      body: SingleChildScrollView(
        child: Padding(
          padding: EdgeInsets.all(20),
          child: Column(
            children: [
              FadeTransition(
                opacity: _fadeAnimation,
                child: animationBox(
                  'Fade Animation',
                  Icons.visibility,
                  Colors.blue,
                ),
              ),
              SizedBox(height: 25),
              SlideTransition(
                position: _slideAnimation,
                child: animationBox(
                  'Slide Animation',
                  Icons.arrow_forward,
                  Colors.green,
                ),
              ),
              SizedBox(height: 25),
              ScaleTransition(
                scale: _scaleAnimation,
                child: animationBox(
                  'Scale Animation',
                  Icons.zoom_in,
                  Colors.orange,
                ),
              ),
              SizedBox(height: 25),
              RotationTransition(
                turns: _rotationAnimation,
                child: animationBox(
                  'Rotation Animation',
                  Icons.rotate_right,
                  Colors.purple,
                ),
              ),
              SizedBox(height: 30),
              ElevatedButton(
                onPressed: restartAnimation,
                child: Text('Replay Animations'),
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

<img width="744" height="575" alt="image" src="https://github.com/user-attachments/assets/eca20af3-b1f1-4f5c-964b-cd0ee6e50066" />

