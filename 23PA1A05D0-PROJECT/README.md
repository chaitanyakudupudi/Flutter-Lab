# BMI Calculator – Flutter Widget Testing

## Aim
To develop a BMI Calculator using Flutter and write widget tests for input validation, button tap, and result text.

## Description
This application calculates Body Mass Index (BMI) using the user's height and weight. It validates the inputs and displays the BMI value with a corresponding category.

## Features
- Simple and user-friendly interface.
- Height input in centimeters.
- Weight input in kilograms.
- Input validation for invalid values.
- BMI calculation and result display.
- Widget tests for validation, button tap, and result text.

## SDG Relevance
**SDG 3: Good Health and Well-being**

The BMI Calculator promotes health awareness by helping users understand their Body Mass Index. Widget testing ensures that input validation and result display work correctly.

## BMI Formula

BMI = Weight (kg) / Height (m)²

Example: Height = 170 cm, Weight = 70 kg

BMI = 70 / (1.7 × 1.7) = 24.2

## Flutter Concepts Used
- `StatefulWidget`
- `TextEditingController`
- `setState()`
- `TextField`
- `ElevatedButton`
- `testWidgets()`
- `tester.enterText()`
- `tester.tap()`
- `expect()`

## Widget Tests
1. **Input Validation:** Checks whether invalid or empty inputs display an error.
2. **Button Tap:** Checks whether tapping the Calculate BMI button displays the result.
3. **Result Text:** Verifies that the correct BMI value and category appear.

## Source Code

### Application Code
```
import 'package:flutter/material.dart';

void main() {
  runApp(const MaterialApp(
    debugShowCheckedModeBanner: false,
    home: BMIApp(),
  ));
}

class BMIApp extends StatefulWidget {
  const BMIApp({super.key});

  @override
  State<BMIApp> createState() => _BMIAppState();
}

class _BMIAppState extends State<BMIApp> {
  final height = TextEditingController();
  final weight = TextEditingController();

  String result = '';
  String error = '';

  void calculateBMI() {
    double? h = double.tryParse(height.text);
    double? w = double.tryParse(weight.text);

    setState(() {
      if (h == null || w == null || h <= 0 || w <= 0) {
        error = 'Please enter valid height and weight';
        result = '';
      } else {
        double bmi = w / ((h / 100) * (h / 100));
        error = '';

        String category = bmi < 18.5
            ? 'Underweight'
            : bmi < 25
                ? 'Normal weight'
                : bmi < 30
                    ? 'Overweight'
                    : 'Obesity';

        result = 'Your BMI: ${bmi.toStringAsFixed(1)}\n$category';
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: const Color(0xFFF1F4FF),
      appBar: AppBar(
        title: const Text('BMI Calculator'),
        centerTitle: true,
        backgroundColor: Colors.indigo,
        foregroundColor: Colors.white,
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(24),
        child: Column(
          children: [
            const Icon(Icons.health_and_safety, size: 85, color: Colors.indigo),
            const Text(
              'Check Your BMI',
              style: TextStyle(fontSize: 26, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 25),
            TextField(
              key: const Key('heightField'),
              controller: height,
              keyboardType: TextInputType.number,
              decoration: const InputDecoration(
                labelText: 'Height (cm)',
                prefixIcon: Icon(Icons.height),
                border: OutlineInputBorder(),
                filled: true,
                fillColor: Colors.white,
              ),
            ),
            const SizedBox(height: 18),
            TextField(
              key: const Key('weightField'),
              controller: weight,
              keyboardType: TextInputType.number,
              decoration: const InputDecoration(
                labelText: 'Weight (kg)',
                prefixIcon: Icon(Icons.monitor_weight),
                border: OutlineInputBorder(),
                filled: true,
                fillColor: Colors.white,
              ),
            ),
            const SizedBox(height: 22),
            SizedBox(
              width: double.infinity,
              height: 50,
              child: ElevatedButton(
                key: const Key('calculateButton'),
                onPressed: calculateBMI,
                style: ElevatedButton.styleFrom(
                  backgroundColor: Colors.indigo,
                  foregroundColor: Colors.white,
                ),
                child: const Text('Calculate BMI'),
              ),
            ),
            if (error.isNotEmpty) ...[
              const SizedBox(height: 18),
              Text(error,
                  key: const Key('errorText'),
                  style: const TextStyle(color: Colors.red)),
            ],
            if (result.isNotEmpty) ...[
              const SizedBox(height: 22),
              Card(
                color: Colors.white,
                child: Padding(
                  padding: const EdgeInsets.all(22),
                  child: Text(
                    result,
                    key: const Key('resultText'),
                    textAlign: TextAlign.center,
                    style: const TextStyle(
                      fontSize: 23,
                      fontWeight: FontWeight.bold,
                      color: Colors.indigo,
                    ),
                  ),
                ),
              ),
            ],
          ],
        ),
      ),
    );
  }
}

```
### Widget Test Code

```
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';
import '../lib/main.dart';

void main() {
  testWidgets('Input validation', (tester) async {
    await tester.pumpWidget(
      const MaterialApp(home: BMIApp()),
    );

    await tester.tap(find.byKey(const Key('calculateButton')));
    await tester.pump();

    expect(find.byKey(const Key('errorText')), findsOneWidget);
    expect(find.text('Please enter valid height and weight'), findsOneWidget);
  });

  testWidgets('Calculate button works', (tester) async {
    await tester.pumpWidget(
      const MaterialApp(home: BMIApp()),
    );

    await tester.enterText(find.byKey(const Key('heightField')), '170');
    await tester.enterText(find.byKey(const Key('weightField')), '70');

    await tester.tap(find.byKey(const Key('calculateButton')));
    await tester.pump();

    expect(find.byKey(const Key('resultText')), findsOneWidget);
  });

  testWidgets('Correct BMI result is displayed', (tester) async {
    await tester.pumpWidget(
      const MaterialApp(home: BMIApp()),
    );

    await tester.enterText(find.byKey(const Key('heightField')), '170');
    await tester.enterText(find.byKey(const Key('weightField')), '70');

    await tester.tap(find.byKey(const Key('calculateButton')));
    await tester.pump();

    expect(find.text('Your BMI: 24.2\nNormal weight'), findsOneWidget);
  });
}

```

## OUTPUT

### Screenshot 1: BMI Calculator Home Screen

<img width="253" height="488" alt="image" src="https://github.com/user-attachments/assets/1ceaa1d2-5d9c-4928-a980-29d43eb86ba5" />


### Screenshot 2: Input Validation

<img width="258" height="490" alt="image" src="https://github.com/user-attachments/assets/3bf4bde6-5b79-4a81-a680-c046c808d78f" />


### Screenshot 3: BMI Calculation Result

<img width="256" height="488" alt="image" src="https://github.com/user-attachments/assets/7cb91b49-6f7f-465f-8064-e816b05b6064" />




## Result
The BMI Calculator was developed successfully using Flutter. Widget tests were prepared to verify input validation, button interaction, and result text.

## Technologies Used
- Flutter
- Dart
- FlutLab

