# 💜 BMI Calculator – Flutter Widget Testing

A modern and interactive **BMI (Body Mass Index) Calculator** developed using Flutter and Dart. The application calculates BMI based on the user's height and weight, displays the corresponding health category, and provides a healthy weight range.

This project also demonstrates Flutter Widget Testing to verify input validation, button interactions, BMI calculations, and result display.

## 📌 Project Overview

- **Project Name:** BMI Calculator
- **Framework:** Flutter
- **Programming Language:** Dart
- **Testing Framework:** Flutter Widget Testing
- **Platform:** Android / Web
- **SDG:** SDG 3 – Good Health and Well-being

## ✨ Features

- Modern and attractive user interface.
- Height input in centimeters.
- Weight input in kilograms.
- BMI calculation using the standard formula.
- Input validation for empty and invalid values.
- Automatic BMI category classification.
- Animated BMI result presentation.
- BMI progress indicator.
- Healthy weight range calculation.
- Reset functionality.
- Responsive user interface.
- Automated widget testing.

## 🧮 BMI Calculation Formula

BMI is calculated using the following formula:

```text
BMI = Weight (kg) / Height (m)²
```

**Example:**

- Height = 170 cm = 1.7 m
- Weight = 70 kg

```text
BMI = 70 / (1.7 × 1.7)
BMI = 24.2
```

### BMI Categories

| BMI Range | Category |
|---|---|
| Below 18.5 | Underweight |
| 18.5 – 24.9 | Normal Weight |
| 25 – 29.9 | Overweight |
| 30 and above | Obesity |

## 🖥️ Application Screenshots

### 1. BMI Calculator Home Screen

<img width="390" height="840" alt="image" src="https://github.com/user-attachments/assets/4f0b631e-7b3f-4651-83d3-d48127d26a83" />



### 2. Input Validation

<img width="386" height="668" alt="image" src="https://github.com/user-attachments/assets/7245ba63-58d2-499f-bdb2-1d44df549f9e" />


### 3. BMI Calculation Result

<img width="382" height="817" alt="image" src="https://github.com/user-attachments/assets/ecb39691-f3ba-4182-89db-ab7fb7dd4c16" />


## 💻 Source Code

### 1. Application Code

**File:** `lib/main.dart`

```
import 'package:flutter/material.dart';

void main() {
  runApp(const BMIApp());
}

const Color bg = Color(0xFF0B0D18);
const Color surface = Color(0xFF171A2B);
const Color purple = Color(0xFF8B72FF);
const Color cyan = Color(0xFF45D6E8);

class BMIApp extends StatelessWidget {
  const BMIApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'BMI Health Dashboard',
      theme: ThemeData(
        brightness: Brightness.dark,
        scaffoldBackgroundColor: bg,
        colorScheme: const ColorScheme.dark(
          primary: purple,
          secondary: cyan,
          surface: surface,
        ),
        useMaterial3: true,
      ),
      home: const BMIHome(),
    );
  }
}

class BMIHome extends StatefulWidget {
  const BMIHome({super.key});

  @override
  State<BMIHome> createState() => _BMIHomeState();
}

class _BMIHomeState extends State<BMIHome> {
  final heightController = TextEditingController();
  final weightController = TextEditingController();
  final chartHeightController = TextEditingController();

  int selectedPage = 0;

  double? bmi;
  double? healthyMin;
  double? healthyMax;
  double? chartHeight;

  String category = '';
  String error = '';

  Color resultColor = cyan;

  @override
  void dispose() {
    heightController.dispose();
    weightController.dispose();
    chartHeightController.dispose();
    super.dispose();
  }

  double? getValidHeight(String value) {
    double? h = double.tryParse(value.trim());

    if (h == null || h < 50 || h > 250) {
      return null;
    }

    return h / 100;
  }

  void calculateBMI() {
    FocusScope.of(context).unfocus();

    double? h = getValidHeight(heightController.text);
    double? w = double.tryParse(weightController.text.trim());

    setState(() {
      if (h == null || w == null || w < 10 || w > 300) {
        error = 'Please enter valid height and weight.';
        bmi = null;
        category = '';
        return;
      }

      bmi = w / (h * h);
      healthyMin = 18.5 * h * h;
      healthyMax = 24.9 * h * h;

      if (bmi! < 18.5) {
        category = 'Underweight';
        resultColor = Colors.lightBlueAccent;
      } else if (bmi! < 25) {
        category = 'Normal Weight';
        resultColor = Colors.greenAccent;
      } else if (bmi! < 30) {
        category = 'Overweight';
        resultColor = Colors.orangeAccent;
      } else {
        category = 'Obesity';
        resultColor = Colors.redAccent;
      }

      error = '';
    });
  }

  void generateChart() {
    FocusScope.of(context).unfocus();

    double? h = getValidHeight(chartHeightController.text);

    setState(() {
      if (h == null) {
        chartHeight = null;
        error = 'Enter a valid height between 50 and 250 cm.';
      } else {
        chartHeight = h;
        error = '';
      }
    });
  }

  void resetCalculator() {
    setState(() {
      heightController.clear();
      weightController.clear();
      chartHeightController.clear();
      bmi = null;
      healthyMin = null;
      healthyMax = null;
      chartHeight = null;
      category = '';
      error = '';
    });
  }

  Widget sectionTitle(String title, String subtitle) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          title,
          style: const TextStyle(
            fontSize: 21,
            fontWeight: FontWeight.bold,
            color: Colors.white,
          ),
        ),
        const SizedBox(height: 5),
        Text(
          subtitle,
          style: const TextStyle(
            color: Colors.white54,
            fontSize: 12,
          ),
        ),
      ],
    );
  }

  Widget darkCard({
    required Widget child,
    EdgeInsets padding = const EdgeInsets.all(20),
  }) {
    return Container(
      width: double.infinity,
      padding: padding,
      decoration: BoxDecoration(
        color: surface,
        borderRadius: BorderRadius.circular(22),
        border: Border.all(color: Colors.white.withOpacity(0.06)),
      ),
      child: child,
    );
  }

  Widget inputField({
    required String label,
    required String hint,
    required IconData icon,
    required TextEditingController controller,
    required String keyName,
  }) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          label,
          style: const TextStyle(
            color: Colors.white70,
            fontSize: 13,
            fontWeight: FontWeight.w600,
          ),
        ),
        const SizedBox(height: 10),
        TextField(
          key: Key(keyName),
          controller: controller,
          keyboardType: const TextInputType.numberWithOptions(
            decimal: true,
          ),
          style: const TextStyle(
            color: Colors.white,
            fontSize: 17,
            fontWeight: FontWeight.w600,
          ),
          decoration: InputDecoration(
            hintText: hint,
            hintStyle: const TextStyle(color: Colors.white30),
            prefixIcon: Icon(icon, color: purple),
            filled: true,
            fillColor: const Color(0xFF22263B),
            contentPadding: const EdgeInsets.all(17),
            border: OutlineInputBorder(
              borderRadius: BorderRadius.circular(15),
              borderSide: BorderSide.none,
            ),
            enabledBorder: OutlineInputBorder(
              borderRadius: BorderRadius.circular(15),
              borderSide: BorderSide(
                color: Colors.white.withOpacity(0.06),
              ),
            ),
            focusedBorder: OutlineInputBorder(
              borderRadius: BorderRadius.circular(15),
              borderSide: const BorderSide(color: purple),
            ),
          ),
        ),
      ],
    );
  }

  Widget primaryButton({
    required String text,
    required IconData icon,
    required VoidCallback onPressed,
    String? keyName,
  }) {
    return SizedBox(
      height: 54,
      width: double.infinity,
      child: ElevatedButton.icon(
        key: keyName == null ? null : Key(keyName),
        onPressed: onPressed,
        icon: Icon(icon),
        label: Text(
          text,
          style: const TextStyle(
            fontWeight: FontWeight.bold,
            fontSize: 15,
          ),
        ),
        style: ElevatedButton.styleFrom(
          backgroundColor: purple,
          foregroundColor: Colors.white,
          elevation: 0,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(15),
          ),
        ),
      ),
    );
  }

  Widget errorMessage() {
    if (error.isEmpty) return const SizedBox.shrink();

    return Container(
      key: const Key('errorText'),
      width: double.infinity,
      padding: const EdgeInsets.all(13),
      margin: const EdgeInsets.only(top: 14),
      decoration: BoxDecoration(
        color: Colors.red.withOpacity(0.12),
        borderRadius: BorderRadius.circular(12),
      ),
      child: Text(
        error,
        style: const TextStyle(
          color: Colors.redAccent,
          fontSize: 12,
        ),
      ),
    );
  }

  Widget buildHeader() {
    return Row(
      children: [
        Container(
          height: 48,
          width: 48,
          decoration: BoxDecoration(
            gradient: const LinearGradient(
              colors: [purple, cyan],
            ),
            borderRadius: BorderRadius.circular(15),
          ),
          child: const Icon(
            Icons.favorite,
            color: Colors.white,
            size: 25,
          ),
        ),
        const SizedBox(width: 13),
        const Expanded(
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Text(
                'HEALTH TRACKER',
                style: TextStyle(
                  fontSize: 18,
                  fontWeight: FontWeight.bold,
                  letterSpacing: 1,
                ),
              ),
              Text(
                'BMI & Wellness Dashboard',
                style: TextStyle(
                  color: Colors.white54,
                  fontSize: 11,
                ),
              ),
            ],
          ),
        ),
        const Icon(Icons.dark_mode, color: purple),
      ],
    );
  }

  Widget buildHero() {
    return Container(
      width: double.infinity,
      padding: const EdgeInsets.all(23),
      decoration: BoxDecoration(
        gradient: const LinearGradient(
          begin: Alignment.topLeft,
          end: Alignment.bottomRight,
          colors: [
            Color(0xFF49368E),
            Color(0xFF252C5A),
            Color(0xFF153C54),
          ],
        ),
        borderRadius: BorderRadius.circular(24),
      ),
      child: const Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Icon(Icons.monitor_heart, color: cyan, size: 32),
          SizedBox(height: 15),
          Text(
            'Understand Your Body',
            style: TextStyle(
              fontSize: 23,
              fontWeight: FontWeight.bold,
              color: Colors.white,
            ),
          ),
          SizedBox(height: 8),
          Text(
            'Calculate your BMI, explore weight ranges, '
            'and track your health indicators.',
            style: TextStyle(
              color: Colors.white70,
              fontSize: 13,
              height: 1.6,
            ),
          ),
        ],
      ),
    );
  }

  Widget buildResult() {
    if (bmi == null) {
      return darkCard(
        child: const Column(
          children: [
            Icon(Icons.analytics_outlined, size: 42, color: purple),
            SizedBox(height: 12),
            Text(
              'Your BMI result will appear here',
              style: TextStyle(
                color: Colors.white70,
                fontWeight: FontWeight.w600,
              ),
            ),
            SizedBox(height: 5),
            Text(
              'Enter your height and weight to get started.',
              textAlign: TextAlign.center,
              style: TextStyle(color: Colors.white38, fontSize: 12),
            ),
          ],
        ),
      );
    }

    return darkCard(
      child: Column(
        children: [
          const Text(
            'YOUR BODY MASS INDEX',
            style: TextStyle(
              color: Colors.white54,
              fontSize: 11,
              letterSpacing: 2,
              fontWeight: FontWeight.bold,
            ),
          ),
          const SizedBox(height: 12),
          Text(
            bmi!.toStringAsFixed(1),
            key: const Key('resultValue'),
            style: TextStyle(
              fontSize: 58,
              fontWeight: FontWeight.bold,
              color: resultColor,
            ),
          ),
          Container(
            key: const Key('resultCategory'),
            padding: const EdgeInsets.symmetric(
              horizontal: 19,
              vertical: 9,
            ),
            decoration: BoxDecoration(
              color: resultColor.withOpacity(0.13),
              borderRadius: BorderRadius.circular(30),
            ),
            child: Text(
              category,
              style: TextStyle(
                color: resultColor,
                fontWeight: FontWeight.bold,
              ),
            ),
          ),
          const SizedBox(height: 25),
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            children: const [
              Text(
                'BMI Progress',
                style: TextStyle(color: Colors.white70),
              ),
              Text(
                '0 - 40+',
                style: TextStyle(color: Colors.white38),
              ),
            ],
          ),
          const SizedBox(height: 12),
          ClipRRect(
            borderRadius: BorderRadius.circular(20),
            child: LinearProgressIndicator(
              value: (bmi! / 40).clamp(0.0, 1.0),
              minHeight: 11,
              backgroundColor: Colors.white12,
              valueColor: AlwaysStoppedAnimation(resultColor),
            ),
          ),
          const SizedBox(height: 25),
          Container(
            padding: const EdgeInsets.all(16),
            decoration: BoxDecoration(
              color: const Color(0xFF22263B),
              borderRadius: BorderRadius.circular(16),
            ),
            child: Row(
              children: [
                const Icon(
                  Icons.monitor_weight_outlined,
                  color: cyan,
                  size: 29,
                ),
                const SizedBox(width: 13),
                Expanded(
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      const Text(
                        'Healthy Weight Range',
                        style: TextStyle(
                          color: Colors.white60,
                          fontSize: 12,
                        ),
                      ),
                      const SizedBox(height: 5),
                      Text(
                        '${healthyMin!.toStringAsFixed(1)} - '
                        '${healthyMax!.toStringAsFixed(1)} kg',
                        key: const Key('healthyRange'),
                        style: const TextStyle(
                          color: cyan,
                          fontWeight: FontWeight.bold,
                          fontSize: 17,
                        ),
                      ),
                    ],
                  ),
                ),
              ],
            ),
          ),
          const SizedBox(height: 15),
          const Text(
            'BMI is a screening measure, not a medical diagnosis.',
            textAlign: TextAlign.center,
            style: TextStyle(
              color: Colors.white38,
              fontSize: 10,
            ),
          ),
        ],
      ),
    );
  }

  Widget categoryRow(
    String name,
    String range,
    Color color,
  ) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 10),
      child: Row(
        children: [
          Container(
            height: 11,
            width: 11,
            decoration: BoxDecoration(
              color: color,
              shape: BoxShape.circle,
            ),
          ),
          const SizedBox(width: 12),
          Expanded(
            child: Text(
              name,
              style: const TextStyle(
                color: Colors.white,
                fontWeight: FontWeight.w600,
              ),
            ),
          ),
          Text(
            range,
            style: const TextStyle(
              color: Colors.white54,
              fontSize: 12,
            ),
          ),
        ],
      ),
    );
  }

  Widget buildReferenceGuide() {
    return darkCard(
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          const Text(
            'BMI Reference Guide',
            style: TextStyle(
              fontSize: 17,
              fontWeight: FontWeight.bold,
            ),
          ),
          const SizedBox(height: 13),
          categoryRow('Underweight', 'Below 18.5', Colors.lightBlueAccent),
          categoryRow('Normal Weight', '18.5 - 24.9', Colors.greenAccent),
          categoryRow('Overweight', '25 - 29.9', Colors.orangeAccent),
          categoryRow('Obesity', '30 and above', Colors.redAccent),
        ],
      ),
    );
  }

  Widget calculatorPage() {
    return SingleChildScrollView(
      padding: const EdgeInsets.fromLTRB(20, 15, 20, 25),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          buildHeader(),
          const SizedBox(height: 25),
          buildHero(),
          const SizedBox(height: 27),
          sectionTitle(
            'Calculate Your BMI',
            'Enter your physical measurements below',
          ),
          const SizedBox(height: 18),
          darkCard(
            child: Column(
              children: [
                inputField(
                  label: 'HEIGHT',
                  hint: 'Enter height in cm',
                  icon: Icons.height,
                  controller: heightController,
                  keyName: 'heightField',
                ),
                const SizedBox(height: 20),
                inputField(
                  label: 'WEIGHT',
                  hint: 'Enter weight in kg',
                  icon: Icons.monitor_weight_outlined,
                  controller: weightController,
                  keyName: 'weightField',
                ),
                errorMessage(),
                const SizedBox(height: 23),
                primaryButton(
                  text: 'Calculate BMI',
                  icon: Icons.calculate_outlined,
                  onPressed: calculateBMI,
                  keyName: 'calculateButton',
                ),
                const SizedBox(height: 12),
                SizedBox(
                  width: double.infinity,
                  height: 48,
                  child: OutlinedButton.icon(
                    key: const Key('resetButton'),
                    onPressed: resetCalculator,
                    icon: const Icon(Icons.refresh),
                    label: const Text('Reset'),
                    style: OutlinedButton.styleFrom(
                      foregroundColor: cyan,
                      side: const BorderSide(color: cyan),
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(15),
                      ),
                    ),
                  ),
                ),
              ],
            ),
          ),
          const SizedBox(height: 25),
          sectionTitle(
            'Your Health Report',
            'Your calculated BMI and healthy weight range',
          ),
          const SizedBox(height: 15),
          buildResult(),
          const SizedBox(height: 23),
          buildReferenceGuide(),
          const SizedBox(height: 20),
          const Center(
            child: Text(
              'SDG 3 • GOOD HEALTH AND WELL-BEING',
              style: TextStyle(
                color: purple,
                fontSize: 10,
                fontWeight: FontWeight.bold,
                letterSpacing: 1,
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget buildChartRow(double bmiValue, String label, Color color) {
    double weight = bmiValue * chartHeight! * chartHeight!;

    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 11),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Row(
            children: [
              Expanded(
                child: Text(
                  label,
                  style: const TextStyle(
                    color: Colors.white,
                    fontWeight: FontWeight.w600,
                  ),
                ),
              ),
              Text(
                '${weight.toStringAsFixed(1)} kg',
                style: TextStyle(
                  color: color,
                  fontWeight: FontWeight.bold,
                ),
              ),
            ],
          ),
          const SizedBox(height: 9),
          ClipRRect(
            borderRadius: BorderRadius.circular(20),
            child: LinearProgressIndicator(
              value: (bmiValue / 40).clamp(0.0, 1.0),
              minHeight: 9,
              backgroundColor: Colors.white12,
              valueColor: AlwaysStoppedAnimation(color),
            ),
          ),
          const SizedBox(height: 4),
          Text(
            'Estimated BMI: ${bmiValue.toStringAsFixed(1)}',
            style: const TextStyle(
              color: Colors.white38,
              fontSize: 11,
            ),
          ),
        ],
      ),
    );
  }

  Widget chartPage() {
    final chartItems = [
      (16.0, 'Underweight', Colors.lightBlueAccent),
      (18.5, 'Normal range starts', Colors.greenAccent),
      (22.0, 'Normal range', Colors.greenAccent),
      (24.9, 'Normal range ends', Colors.greenAccent),
      (27.5, 'Overweight', Colors.orangeAccent),
      (30.0, 'Obesity', Colors.redAccent),
      (35.0, 'Higher BMI', Colors.redAccent),
      (40.0, 'Higher BMI', Colors.redAccent),
    ];

    return SingleChildScrollView(
      padding: const EdgeInsets.fromLTRB(20, 15, 20, 25),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          buildHeader(),
          const SizedBox(height: 28),
          sectionTitle(
            'BMI Weight Estimator',
            'Explore how different weights affect your BMI',
          ),
          const SizedBox(height: 20),
          darkCard(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                const Text(
                  'Enter Your Height',
                  style: TextStyle(
                    color: Colors.white70,
                    fontWeight: FontWeight.w600,
                  ),
                ),
                const SizedBox(height: 12),
                inputField(
                  label: 'HEIGHT (CM)',
                  hint: 'Example: 170',
                  icon: Icons.height,
                  controller: chartHeightController,
                  keyName: 'chartHeightField',
                ),
                errorMessage(),
                const SizedBox(height: 18),
                primaryButton(
                  text: 'Generate BMI Chart',
                  icon: Icons.bar_chart,
                  onPressed: generateChart,
                  keyName: 'generateChartButton',
                ),
              ],
            ),
          ),
          const SizedBox(height: 22),
          if (chartHeight != null) ...[
            darkCard(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  const Text(
                    'ESTIMATED BMI BY WEIGHT',
                    style: TextStyle(
                      color: cyan,
                      fontWeight: FontWeight.bold,
                      letterSpacing: 1,
                      fontSize: 12,
                    ),
                  ),
                  const SizedBox(height: 7),
                  Text(
                    'Height: ${(chartHeight! * 100).toStringAsFixed(0)} cm',
                    style: const TextStyle(
                      color: Colors.white54,
                      fontSize: 12,
                    ),
                  ),
                  const SizedBox(height: 15),
                  ...chartItems.map(
                    (item) => buildChartRow(
                      item.$1,
                      item.$2,
                      item.$3,
                    ),
                  ),
                ],
              ),
            ),
            const SizedBox(height: 20),
            darkCard(
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.start,
                children: [
                  const Text(
                    'HEALTHY WEIGHT ESTIMATE',
                    style: TextStyle(
                      color: cyan,
                      fontWeight: FontWeight.bold,
                      letterSpacing: 1,
                      fontSize: 12,
                    ),
                  ),
                  const SizedBox(height: 15),
                  Text(
                    '${(18.5 * chartHeight! * chartHeight!).toStringAsFixed(1)} '
                    '- ${(24.9 * chartHeight! * chartHeight!).toStringAsFixed(1)} kg',
                    key: const Key('chartHealthyRange'),
                    style: const TextStyle(
                      fontSize: 23,
                      fontWeight: FontWeight.bold,
                      color: Colors.greenAccent,
                    ),
                  ),
                  const SizedBox(height: 8),
                  const Text(
                    'Estimated weight range corresponding to a BMI '
                    'of 18.5 to 24.9.',
                    style: TextStyle(
                      color: Colors.white54,
                      fontSize: 12,
                      height: 1.5,
                    ),
                  ),
                ],
              ),
            ),
          ] else
            darkCard(
              child: const Column(
                children: [
                  Icon(
                    Icons.insert_chart_outlined,
                    size: 45,
                    color: purple,
                  ),
                  SizedBox(height: 12),
                  Text(
                    'Your BMI chart will appear here',
                    style: TextStyle(
                      color: Colors.white70,
                      fontWeight: FontWeight.w600,
                    ),
                  ),
                  SizedBox(height: 6),
                  Text(
                    'Enter your height and tap Generate BMI Chart.',
                    textAlign: TextAlign.center,
                    style: TextStyle(
                      color: Colors.white38,
                      fontSize: 12,
                    ),
                  ),
                ],
              ),
            ),
          const SizedBox(height: 20),
          buildReferenceGuide(),
          const SizedBox(height: 20),
          const Center(
            child: Text(
              'ESTIMATES ARE FOR GENERAL INFORMATION ONLY',
              style: TextStyle(
                color: Colors.white38,
                fontSize: 9,
                letterSpacing: 0.7,
              ),
            ),
          ),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SafeArea(
        child: IndexedStack(
          index: selectedPage,
          children: [
            calculatorPage(),
            chartPage(),
          ],
        ),
      ),
      bottomNavigationBar: NavigationBar(
        key: const Key('bottomNavigation'),
        selectedIndex: selectedPage,
        onDestinationSelected: (index) {
          setState(() {
            selectedPage = index;
            error = '';
          });
        },
        backgroundColor: const Color(0xFF171A2B),
        indicatorColor: purple.withOpacity(0.2),
        destinations: const [
          NavigationDestination(
            icon: Icon(Icons.calculate_outlined),
            selectedIcon: Icon(Icons.calculate, color: purple),
            label: 'Calculator',
          ),
          NavigationDestination(
            icon: Icon(Icons.bar_chart_outlined),
            selectedIcon: Icon(Icons.bar_chart, color: purple),
            label: 'BMI Chart',
          ),
        ],
      ),
    );
  }
}

```


### 2. Widget Test Code

**File:** `test/widget_test.dart`

```

import 'package:flutter_test/flutter_test.dart';
import 'package:flutter/material.dart';
import '../lib/main.dart';

void main() {
  group('BMI Health Dashboard Widget Tests', () {

    testWidgets('Test 1: Empty input validation',
        (WidgetTester tester) async {
      await tester.pumpWidget(const BMIApp());

      await tester.ensureVisible(
        find.byKey(const Key('calculateButton')),
      );

      await tester.tap(
        find.byKey(const Key('calculateButton')),
      );

      await tester.pumpAndSettle();

      expect(find.byKey(const Key('errorText')), findsOneWidget);
      expect(find.textContaining('valid height'), findsOneWidget);
    });

    testWidgets('Test 2: Invalid input validation',
        (WidgetTester tester) async {
      await tester.pumpWidget(const BMIApp());

      await tester.enterText(
        find.byKey(const Key('heightField')),
        '0',
      );

      await tester.enterText(
        find.byKey(const Key('weightField')),
        '70',
      );

      await tester.ensureVisible(
        find.byKey(const Key('calculateButton')),
      );

      await tester.tap(
        find.byKey(const Key('calculateButton')),
      );

      await tester.pumpAndSettle();

      expect(find.byKey(const Key('errorText')), findsOneWidget);
      expect(find.byKey(const Key('resultValue')), findsNothing);
    });

    testWidgets('Test 3: Calculate BMI button functionality',
        (WidgetTester tester) async {
      await tester.pumpWidget(const BMIApp());

      await tester.enterText(
        find.byKey(const Key('heightField')),
        '170',
      );

      await tester.enterText(
        find.byKey(const Key('weightField')),
        '70',
      );

      await tester.ensureVisible(
        find.byKey(const Key('calculateButton')),
      );

      await tester.tap(
        find.byKey(const Key('calculateButton')),
      );

      await tester.pumpAndSettle();

      expect(find.byKey(const Key('resultValue')), findsOneWidget);
    });

    testWidgets('Test 4: Correct BMI value and category',
        (WidgetTester tester) async {
      await tester.pumpWidget(const BMIApp());

      await tester.enterText(
        find.byKey(const Key('heightField')),
        '170',
      );

      await tester.enterText(
        find.byKey(const Key('weightField')),
        '70',
      );

      await tester.ensureVisible(
        find.byKey(const Key('calculateButton')),
      );

      await tester.tap(
        find.byKey(const Key('calculateButton')),
      );

      await tester.pumpAndSettle();

      expect(find.text('24.2'), findsOneWidget);
      expect(find.text('Normal Weight'), findsOneWidget);
      expect(find.byKey(const Key('healthyRange')), findsOneWidget);
    });

    testWidgets('Test 5: Reset button functionality',
        (WidgetTester tester) async {
      await tester.pumpWidget(const BMIApp());

      await tester.enterText(
        find.byKey(const Key('heightField')),
        '170',
      );

      await tester.enterText(
        find.byKey(const Key('weightField')),
        '70',
      );

      await tester.ensureVisible(
        find.byKey(const Key('calculateButton')),
      );

      await tester.tap(
        find.byKey(const Key('calculateButton')),
      );

      await tester.pumpAndSettle();

      await tester.ensureVisible(
        find.byKey(const Key('resetButton')),
      );

      await tester.tap(
        find.byKey(const Key('resetButton')),
      );

      await tester.pumpAndSettle();

      expect(find.byKey(const Key('resultValue')), findsNothing);
      expect(find.byKey(const Key('healthyRange')), findsNothing);

      expect(
        tester.widget<TextField>(
          find.byKey(const Key('heightField')),
        ).controller!.text,
        '',
      );

      expect(
        tester.widget<TextField>(
          find.byKey(const Key('weightField')),
        ).controller!.text,
        '',
      );
    });

    testWidgets('Test 6: BMI Chart navigation',
        (WidgetTester tester) async {
      await tester.pumpWidget(const BMIApp());

      await tester.tap(
        find.byIcon(Icons.bar_chart_outlined),
      );

      await tester.pumpAndSettle();

      expect(find.text('BMI Weight Estimator'), findsOneWidget);
      expect(find.byKey(const Key('chartHeightField')), findsOneWidget);
    });

    testWidgets('Test 7: Generate BMI weight chart',
        (WidgetTester tester) async {
      await tester.pumpWidget(const BMIApp());

      await tester.tap(
        find.byIcon(Icons.bar_chart_outlined),
      );

      await tester.pumpAndSettle();

      await tester.enterText(
        find.byKey(const Key('chartHeightField')),
        '170',
      );

      await tester.ensureVisible(
        find.byKey(const Key('generateChartButton')),
      );

      await tester.tap(
        find.byKey(const Key('generateChartButton')),
      );

      await tester.pumpAndSettle();

      expect(find.byKey(const Key('chartHealthyRange')), findsOneWidget);
      expect(find.text('Estimated BMI: 18.5'), findsOneWidget);
      expect(find.text('Estimated BMI: 24.9'), findsOneWidget);
    });
  });
}

```



## 🧪 Widget Testing

Flutter's `flutter_test` package is used to verify the functionality and reliability of the application.

### Test Cases

| Test Case | Description |
|---|---|
| Test 1 | Validates empty input fields |
| Test 2 | Validates invalid input values |
| Test 3 | Checks Calculate BMI button functionality |
| Test 4 | Verifies BMI value and category |
| Test 5 | Checks Reset button functionality |

### Testing Methods Used

- `testWidgets()`
- `pumpWidget()`
- `enterText()`
- `tap()`
- `pumpAndSettle()`
- `find.byKey()`
- `expect()`

### Expected Test Output

```text
All widget tests passed!
```

## 🛠️ Technologies Used

- Flutter
- Dart
- Material 3
- Flutter Test
- Visual Studio Code

## 📂 Project Structure

```text
bmi_calculator/
│
├── lib/
│   └── main.dart
│
├── test/
│   └── widget_test.dart
│
├── android/
├── web/
├── pubspec.yaml
├── analysis_options.yaml
└── README.md
```

## 🌍 SDG Relevance

### SDG 3 – Good Health and Well-being

This project promotes health awareness by helping users calculate their Body Mass Index and understand its corresponding category.

Automated widget testing improves application reliability by verifying input validation, user interactions, and result accuracy.

*Note: BMI is a general screening measure and should not be considered a medical diagnosis.*

## 🎯 Learning Outcomes

- Understanding StatefulWidget and setState().
- Working with TextEditingController.
- Implementing TextField and ElevatedButton.
- Performing input validation.
- Implementing BMI calculations using Dart.
- Developing responsive Flutter interfaces.
- Writing and executing Flutter widget tests.
- Verifying application functionality using test assertions.

