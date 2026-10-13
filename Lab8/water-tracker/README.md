# Water Intake Tracker – Flutter

## Source Code

```dart
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';

void main() {
  runApp(const WaterTrackerApp());
}

class WaterTrackerApp extends StatelessWidget {
  const WaterTrackerApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Water Intake Tracker',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(
          seedColor: Colors.blue,
        ),
        useMaterial3: true,
        scaffoldBackgroundColor: const Color(0xFFF4F8FC),
      ),
      home: const HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  int waterIntake = 0;
  int dailyGoal = 2000;
  bool isLoading = true;

  String get today =>
      '${DateTime.now().year}-${DateTime.now().month.toString().padLeft(2, '0')}-${DateTime.now().day.toString().padLeft(2, '0')}';

  @override
  void initState() {
    super.initState();
    loadData();
  }

  Future<void> loadData() async {
    try {
      final prefs = await SharedPreferences.getInstance();

      dailyGoal = prefs.getInt('dailyGoal') ?? 2000;

      final savedDate = prefs.getString('lastSavedDate');

      if (savedDate != today) {
        waterIntake = 0;
        await prefs.setInt('waterIntake', 0);
        await prefs.setString('lastSavedDate', today);
      } else {
        waterIntake = prefs.getInt('waterIntake') ?? 0;
      }
    } catch (e) {
      waterIntake = 0;
      dailyGoal = 2000;
    }

    if (mounted) {
      setState(() {
        isLoading = false;
      });
    }
  }

  Future<void> saveData() async {
    try {
      final prefs = await SharedPreferences.getInstance();
      await prefs.setInt('waterIntake', waterIntake);
      await prefs.setInt('dailyGoal', dailyGoal);
      await prefs.setString('lastSavedDate', today);
    } catch (e) {
      debugPrint('Error saving data: $e');
    }
  }

  void addWater(int amount) {
    setState(() {
      waterIntake += amount;
    });

    unawaited(saveData());
  }

  void resetWater() {
    setState(() {
      waterIntake = 0;
    });

    unawaited(saveData());
  }

  Future<void> openAddWaterPage() async {
    await Navigator.push(
      context,
      MaterialPageRoute(
        builder: (_) => AddWaterPage(
          onAddWater: addWater,
        ),
      ),
    );
  }

  Future<void> openGoalPage() async {
    await Navigator.push(
      context,
      MaterialPageRoute(
        builder: (_) => GoalPage(
          currentGoal: dailyGoal,
          onGoalChanged: (goal) {
            setState(() {
              dailyGoal = goal;
            });
            unawaited(saveData());
          },
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    if (isLoading) {
      return const Scaffold(
        body: Center(child: CircularProgressIndicator()),
      );
    }

    final double progress =
        (waterIntake / dailyGoal).clamp(0.0, 1.0).toDouble();

    final bool goalReached = waterIntake >= dailyGoal;

    return Scaffold(
      appBar: AppBar(
        title: const Text(
          'Water Tracker',
          style: TextStyle(fontWeight: FontWeight.bold),
        ),
        centerTitle: true,
        backgroundColor: Colors.white,
        actions: [
          IconButton(
            tooltip: 'Daily goal settings',
            onPressed: openGoalPage,
            icon: const Icon(Icons.settings_outlined),
          ),
        ],
      ),
      body: SafeArea(
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(20),
          child: Column(
            children: [
              const SizedBox(height: 10),
              const Text(
                'Stay Hydrated!',
                style: TextStyle(
                  fontSize: 27,
                  fontWeight: FontWeight.bold,
                ),
              ),
              const SizedBox(height: 8),
              Text(
                'Small sips, healthier days.',
                style: TextStyle(
                  fontSize: 16,
                  color: Colors.grey.shade600,
                ),
              ),
              const SizedBox(height: 25),
              Container(
                width: double.infinity,
                padding: const EdgeInsets.all(24),
                decoration: BoxDecoration(
                  color: Colors.white,
                  borderRadius: BorderRadius.circular(25),
                  boxShadow: [
                    BoxShadow(
                      color: Colors.blue.withValues(alpha: 0.08),
                      blurRadius: 18,
                      offset: const Offset(0, 6),
                    ),
                  ],
                ),
                child: Column(
                  children: [
                    const Icon(
                      Icons.water_drop_rounded,
                      size: 75,
                      color: Colors.blue,
                    ),
                    const SizedBox(height: 12),
                    const Text(
                      "Today's Water Intake",
                      style: TextStyle(fontSize: 18),
                    ),
                    const SizedBox(height: 8),
                    Text(
                      '$waterIntake ml',
                      style: const TextStyle(
                        fontSize: 38,
                        fontWeight: FontWeight.bold,
                        color: Colors.blue,
                      ),
                    ),
                    const SizedBox(height: 8),
                    Text(
                      'Daily goal: $dailyGoal ml',
                      style: TextStyle(
                        fontSize: 16,
                        color: Colors.grey.shade700,
                      ),
                    ),
                    const SizedBox(height: 25),
                    ClipRRect(
                      borderRadius: BorderRadius.circular(20),
                      child: LinearProgressIndicator(
                        value: progress,
                        minHeight: 14,
                        backgroundColor: Colors.blue.shade50,
                      ),
                    ),
                    const SizedBox(height: 12),
                    Text(
                      goalReached
                          ? 'Daily goal reached! Great job!'
                          : '${(progress * 100).round()}% completed',
                      style: TextStyle(
                        fontSize: 16,
                        fontWeight: FontWeight.w600,
                        color: goalReached ? Colors.green : Colors.blue,
                      ),
                    ),
                  ],
                ),
              ),
              const SizedBox(height: 25),
              const Align(
                alignment: Alignment.centerLeft,
                child: Text(
                  'Quick Add Water',
                  style: TextStyle(
                    fontSize: 20,
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ),
              const SizedBox(height: 15),
              Row(
                children: [
                  Expanded(child: quickAddButton(250)),
                  const SizedBox(width: 10),
                  Expanded(child: quickAddButton(500)),
                  const SizedBox(width: 10),
                  Expanded(child: quickAddButton(750)),
                ],
              ),
              const SizedBox(height: 18),
              SizedBox(
                width: double.infinity,
                height: 52,
                child: ElevatedButton.icon(
                  onPressed: openAddWaterPage,
                  icon: const Icon(Icons.add_circle_outline),
                  label: const Text(
                    'Custom Water Amount',
                    style: TextStyle(fontSize: 16),
                  ),
                  style: ElevatedButton.styleFrom(
                    backgroundColor: Colors.blue,
                    foregroundColor: Colors.white,
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(15),
                    ),
                  ),
                ),
              ),
              const SizedBox(height: 12),
              SizedBox(
                width: double.infinity,
                height: 50,
                child: OutlinedButton.icon(
                  onPressed: resetWater,
                  icon: const Icon(Icons.refresh),
                  label: const Text('Reset Today'),
                  style: OutlinedButton.styleFrom(
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(15),
                    ),
                  ),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget quickAddButton(int amount) {
    return ElevatedButton(
      onPressed: () => addWater(amount),
      style: ElevatedButton.styleFrom(
        padding: const EdgeInsets.symmetric(vertical: 16),
        backgroundColor: Colors.blue.shade50,
        foregroundColor: Colors.blue.shade800,
        elevation: 0,
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(14),
        ),
      ),
      child: Text(
        '+$amount ml',
        style: const TextStyle(fontWeight: FontWeight.bold),
      ),
    );
  }
}

// Add Water Page
class AddWaterPage extends StatefulWidget {
  final void Function(int) onAddWater;

  const AddWaterPage({
    super.key,
    required this.onAddWater,
  });

  @override
  State<AddWaterPage> createState() => _AddWaterPageState();
}

class _AddWaterPageState extends State<AddWaterPage> {
  final TextEditingController waterController = TextEditingController();

  @override
  void dispose() {
    waterController.dispose();
    super.dispose();
  }

  void addWater() {
    final amount = int.tryParse(waterController.text.trim());

    if (amount == null || amount <= 0) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Please enter a valid amount in ml.'),
        ),
      );
      return;
    }

    widget.onAddWater(amount);
    Navigator.pop(context);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Add Water')),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(24),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            const Icon(
              Icons.water_drop,
              size: 85,
              color: Colors.blue,
            ),
            const SizedBox(height: 15),
            const Text(
              'Add Your Water Intake',
              textAlign: TextAlign.center,
              style: TextStyle(
                fontSize: 23,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 25),
            TextField(
              controller: waterController,
              keyboardType: TextInputType.number,
              decoration: InputDecoration(
                labelText: 'Amount in ml',
                hintText: 'Example: 300',
                prefixIcon: const Icon(Icons.water_drop),
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(15),
                ),
              ),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: addWater,
              style: ElevatedButton.styleFrom(
                padding: const EdgeInsets.all(16),
              ),
              child: const Text('Add Water'),
            ),
            const SizedBox(height: 25),
            const Text(
              'Choose a quick amount',
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 12),
            Wrap(
              spacing: 10,
              runSpacing: 10,
              children: [250, 500, 750].map((amount) {
                return ActionChip(
                  avatar: const Icon(
                    Icons.water_drop,
                    size: 18,
                  ),
                  label: Text('$amount ml'),
                  onPressed: () {
                    waterController.text = amount.toString();
                  },
                );
              }).toList(),
            ),
          ],
        ),
      ),
    );
  }
}

// Daily Goal Page
class GoalPage extends StatefulWidget {
  final int currentGoal;
  final void Function(int) onGoalChanged;

  const GoalPage({
    super.key,
    required this.currentGoal,
    required this.onGoalChanged,
  });

  @override
  State<GoalPage> createState() => _GoalPageState();
}

class _GoalPageState extends State<GoalPage> {
  late final TextEditingController goalController;

  @override
  void initState() {
    super.initState();
    goalController = TextEditingController(
      text: widget.currentGoal.toString(),
    );
  }

  @override
  void dispose() {
    goalController.dispose();
    super.dispose();
  }

  void saveGoal() {
    final goal = int.tryParse(goalController.text.trim());

    if (goal == null || goal <= 0) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(
          content: Text('Please enter a valid daily goal.'),
        ),
      );
      return;
    }

    widget.onGoalChanged(goal);
    Navigator.pop(context);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Daily Goal')),
      body: Padding(
        padding: const EdgeInsets.all(24),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            const Icon(
              Icons.flag_circle,
              size: 85,
              color: Colors.blue,
            ),
            const SizedBox(height: 20),
            const Text(
              'Set Your Daily Water Goal',
              textAlign: TextAlign.center,
              style: TextStyle(
                fontSize: 23,
                fontWeight: FontWeight.bold,
              ),
            ),
            const SizedBox(height: 25),
            TextField(
              controller: goalController,
              keyboardType: TextInputType.number,
              decoration: InputDecoration(
                labelText: 'Daily goal in ml',
                hintText: 'Example: 2000',
                prefixIcon: const Icon(Icons.flag),
                border: OutlineInputBorder(
                  borderRadius: BorderRadius.circular(15),
                ),
              ),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: saveGoal,
              style: ElevatedButton.styleFrom(
                padding: const EdgeInsets.all(16),
              ),
              child: const Text('Save Goal'),
            ),
          ],
        ),
      ),
    );
  }
}
```

## OUTPUT

<img width="929" height="596" alt="image" src="https://github.com/user-attachments/assets/de119371-5c27-4553-9e14-cb4695ce4e94" />

<img width="922" height="591" alt="image" src="https://github.com/user-attachments/assets/125f4f0c-18b4-4b2d-a1a3-67cab0e14ae2" />

<img width="922" height="598" alt="image" src="https://github.com/user-attachments/assets/07f488ec-0284-4b54-82c1-b6fa9113994a" />
