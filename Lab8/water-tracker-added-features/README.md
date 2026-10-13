# HydroTrack – Water Intake Tracker (Flutter)

## Source Code

```dart
import 'dart:async';
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';

void main() {
  runApp(const WaterApp());
}

class WaterApp extends StatelessWidget {
  const WaterApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'HydroTrack',
      theme: ThemeData(
        useMaterial3: true,
        colorSchemeSeed: Colors.blue,
        scaffoldBackgroundColor: const Color(0xffF5FAFF),
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
  int water = 0;
  int goal = 2000;
  int entries = 0;
  bool isLoading = true;

  Future<void> _saveQueue = Future<void>.value();

  String get today {
    final now = DateTime.now();
    return '${now.year}-${now.month.toString().padLeft(2, '0')}-${now.day.toString().padLeft(2, '0')}';
  }

  @override
  void initState() {
    super.initState();
    loadData();
  }

  Future<void> loadData() async {
    try {
      final prefs = await SharedPreferences.getInstance();

      final savedDate = prefs.getString('lastDate');

      int loadedWater = prefs.getInt('water') ?? 0;
      int loadedGoal = prefs.getInt('goal') ?? 2000;
      int loadedEntries = prefs.getInt('entries') ?? 0;

      if (loadedGoal <= 0) loadedGoal = 2000;

      if (savedDate != today) {
        loadedWater = 0;
        loadedEntries = 0;

        await prefs.setInt('water', 0);
        await prefs.setInt('entries', 0);
        await prefs.setString('lastDate', today);
      }

      if (!mounted) return;

      setState(() {
        water = loadedWater;
        goal = loadedGoal;
        entries = loadedEntries;
        isLoading = false;
      });
    } catch (e) {
      if (!mounted) return;

      setState(() {
        isLoading = false;
      });

      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Could not load saved data.')),
      );
    }
  }

  Future<void> saveData() {
    final savedWater = water;
    final savedGoal = goal;
    final savedEntries = entries;
    final savedDate = today;

    _saveQueue = _saveQueue.then((_) async {
      final prefs = await SharedPreferences.getInstance();

      await prefs.setInt('water', savedWater);
      await prefs.setInt('goal', savedGoal);
      await prefs.setInt('entries', savedEntries);
      await prefs.setString('lastDate', savedDate);
    });

    return _saveQueue;
  }

  Future<void> checkNewDay() async {
    final prefs = await SharedPreferences.getInstance();

    if (prefs.getString('lastDate') != today) {
      if (!mounted) return;

      setState(() {
        water = 0;
        entries = 0;
      });

      await saveData();
    }
  }

  Future<void> addWater(int amount) async {
    if (amount <= 0) return;

    try {
      await checkNewDay();
      if (!mounted) return;

      setState(() {
        water += amount;
        entries++;
      });

      await saveData();
    } catch (e) {
      if (!mounted) return;

      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Could not save water intake.')),
      );
    }
  }

  Future<void> setGoal(int newGoal) async {
    if (newGoal <= 0) return;

    try {
      setState(() {
        goal = newGoal;
      });

      await saveData();
    } catch (e) {
      if (!mounted) return;

      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Could not save daily goal.')),
      );
    }
  }

  Future<void> resetWater() async {
    try {
      setState(() {
        water = 0;
        entries = 0;
      });

      await saveData();
    } catch (e) {
      if (!mounted) return;

      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Could not reset water intake.')),
      );
    }
  }

  Future<void> showWaterDialog() async {
    final controller = TextEditingController();

    await showDialog<void>(
      context: context,
      builder: (dialogContext) {
        return AlertDialog(
          title: const Text(
            'Add Water',
            style: TextStyle(fontWeight: FontWeight.bold),
          ),
          content: TextField(
            controller: controller,
            autofocus: true,
            keyboardType: TextInputType.number,
            decoration: InputDecoration(
              labelText: 'Amount in ml',
              hintText: 'Example: 300',
              prefixIcon: const Icon(Icons.water_drop),
              border: OutlineInputBorder(
                borderRadius: BorderRadius.circular(12),
              ),
            ),
          ),
          actions: [
            TextButton(
              onPressed: () => Navigator.pop(dialogContext),
              child: const Text('Cancel'),
            ),
            ElevatedButton(
              onPressed: () async {
                final amount = int.tryParse(controller.text.trim());

                if (amount == null || amount <= 0) {
                  ScaffoldMessenger.of(context).showSnackBar(
                    const SnackBar(
                      content: Text('Enter a valid amount greater than 0 ml.'),
                    ),
                  );
                  return;
                }

                await addWater(amount);

                if (dialogContext.mounted) {
                  Navigator.pop(dialogContext);
                }
              },
              child: const Text('Add'),
            ),
          ],
        );
      },
    );

    controller.dispose();
  }

  Future<void> showGoalDialog() async {
    final controller = TextEditingController(text: goal.toString());

    await showDialog<void>(
      context: context,
      builder: (dialogContext) {
        return AlertDialog(
          title: const Text(
            'Set Daily Goal',
            style: TextStyle(fontWeight: FontWeight.bold),
          ),
          content: TextField(
            controller: controller,
            keyboardType: TextInputType.number,
            decoration: InputDecoration(
              labelText: 'Daily goal in ml',
              hintText: 'Example: 2000',
              prefixIcon: const Icon(Icons.flag),
              border: OutlineInputBorder(
                borderRadius: BorderRadius.circular(12),
              ),
            ),
          ),
          actions: [
            TextButton(
              onPressed: () => Navigator.pop(dialogContext),
              child: const Text('Cancel'),
            ),
            ElevatedButton(
              onPressed: () async {
                final newGoal = int.tryParse(controller.text.trim());

                if (newGoal == null || newGoal <= 0) {
                  ScaffoldMessenger.of(context).showSnackBar(
                    const SnackBar(
                      content: Text('Goal must be greater than 0 ml.'),
                    ),
                  );
                  return;
                }

                await setGoal(newGoal);

                if (dialogContext.mounted) {
                  Navigator.pop(dialogContext);
                }
              },
              child: const Text('Save'),
            ),
          ],
        );
      },
    );

    controller.dispose();
  }

  void showResetDialog() {
    showDialog<void>(
      context: context,
      builder: (dialogContext) {
        return AlertDialog(
          title: const Text('Reset Water Intake?'),
          content: const Text(
            'Are you sure you want to reset today\'s water intake?',
          ),
          actions: [
            TextButton(
              onPressed: () => Navigator.pop(dialogContext),
              child: const Text('Cancel'),
            ),
            ElevatedButton(
              style: ElevatedButton.styleFrom(
                backgroundColor: Colors.red,
                foregroundColor: Colors.white,
              ),
              onPressed: () async {
                await resetWater();

                if (dialogContext.mounted) {
                  Navigator.pop(dialogContext);
                }
              },
              child: const Text('Reset'),
            ),
          ],
        );
      },
    );
  }

  Widget waterButton(String title, IconData icon, int? amount) {
    return SizedBox(
      height: 65,
      child: ElevatedButton(
        onPressed: () {
          if (amount != null) {
            unawaited(addWater(amount));
          } else {
            showWaterDialog();
          }
        },
        style: ElevatedButton.styleFrom(
          backgroundColor: Colors.white,
          foregroundColor: Colors.blue,
          elevation: 2,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(16),
          ),
        ),
        child: Row(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Icon(icon),
            const SizedBox(width: 8),
            Flexible(
              child: Text(
                title,
                style: const TextStyle(fontWeight: FontWeight.bold),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget statCard(IconData icon, String value, String title) {
    return Container(
      padding: const EdgeInsets.symmetric(vertical: 15, horizontal: 5),
      decoration: BoxDecoration(
        color: Colors.white,
        borderRadius: BorderRadius.circular(16),
        boxShadow: [
          BoxShadow(
            color: Colors.black.withOpacity(0.05),
            blurRadius: 8,
          ),
        ],
      ),
      child: Column(
        children: [
          Icon(icon, color: Colors.blue, size: 25),
          const SizedBox(height: 7),
          FittedBox(
            fit: BoxFit.scaleDown,
            child: Text(
              value,
              style: const TextStyle(
                fontWeight: FontWeight.bold,
                fontSize: 15,
              ),
            ),
          ),
          const SizedBox(height: 3),
          Text(
            title,
            textAlign: TextAlign.center,
            style: const TextStyle(color: Colors.grey, fontSize: 11),
          ),
        ],
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
        (water / goal).clamp(0.0, 1.0).toDouble();

    final int percentage = (progress * 100).round();
    final int remaining = (goal - water).clamp(0, goal);

    return Scaffold(
      appBar: AppBar(
        title: const Text(
          'HydroTrack 💧',
          style: TextStyle(fontWeight: FontWeight.bold),
        ),
        backgroundColor: Colors.transparent,
        elevation: 0,
        actions: [
          IconButton(
            tooltip: 'About HydroTrack',
            icon: const Icon(Icons.info_outline),
            onPressed: () {
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (_) => const AboutPage(),
                ),
              );
            },
          ),
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(20),
        child: Column(
          children: [
            const Align(
              alignment: Alignment.centerLeft,
              child: Text(
                'Stay Hydrated!',
                style: TextStyle(
                  fontSize: 28,
                  fontWeight: FontWeight.bold,
                  color: Color(0xff123B63),
                ),
              ),
            ),
            const SizedBox(height: 5),
            const Align(
              alignment: Alignment.centerLeft,
              child: Text(
                'Track your daily water intake',
                style: TextStyle(fontSize: 15, color: Colors.grey),
              ),
            ),
            const SizedBox(height: 25),
            Container(
              width: double.infinity,
              padding: const EdgeInsets.all(25),
              decoration: BoxDecoration(
                gradient: const LinearGradient(
                  colors: [
                    Color(0xff1976D2),
                    Color(0xff42A5F5),
                  ],
                  begin: Alignment.topLeft,
                  end: Alignment.bottomRight,
                ),
                borderRadius: BorderRadius.circular(28),
                boxShadow: [
                  BoxShadow(
                    color: Colors.blue.withOpacity(0.25),
                    blurRadius: 15,
                    offset: const Offset(0, 8),
                  ),
                ],
              ),
              child: Column(
                children: [
                  const Text(
                    'Today\'s Progress',
                    style: TextStyle(color: Colors.white, fontSize: 18),
                  ),
                  const SizedBox(height: 20),
                  Stack(
                    alignment: Alignment.center,
                    children: [
                      SizedBox(
                        width: 180,
                        height: 180,
                        child: CircularProgressIndicator(
                          value: progress,
                          strokeWidth: 16,
                          backgroundColor: Colors.white24,
                          valueColor:
                              const AlwaysStoppedAnimation<Color>(
                            Colors.white,
                          ),
                        ),
                      ),
                      Column(
                        children: [
                          Text(
                            '$water ml',
                            style: const TextStyle(
                              color: Colors.white,
                              fontSize: 30,
                              fontWeight: FontWeight.bold,
                            ),
                          ),
                          Text(
                            'of $goal ml',
                            style: const TextStyle(
                              color: Colors.white70,
                              fontSize: 14,
                            ),
                          ),
                        ],
                      ),
                    ],
                  ),
                  const SizedBox(height: 20),
                  Text(
                    '$percentage% Completed',
                    style: const TextStyle(
                      color: Colors.white,
                      fontSize: 18,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                  const SizedBox(height: 8),
                  Text(
                    remaining == 0
                        ? '🎉 Goal Completed!'
                        : '$remaining ml remaining',
                    style: const TextStyle(
                      color: Colors.white,
                      fontSize: 15,
                    ),
                  ),
                ],
              ),
            ),
            const SizedBox(height: 20),
            SizedBox(
              width: double.infinity,
              height: 55,
              child: ElevatedButton.icon(
                onPressed: showGoalDialog,
                icon: const Icon(Icons.flag),
                label: Text(
                  'Daily Goal: $goal ml  •  Modify',
                  style: const TextStyle(
                    fontSize: 16,
                    fontWeight: FontWeight.bold,
                  ),
                ),
                style: ElevatedButton.styleFrom(
                  backgroundColor: Colors.blue.shade50,
                  foregroundColor: Colors.blue.shade800,
                  elevation: 0,
                  shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(16),
                  ),
                ),
              ),
            ),
            const SizedBox(height: 25),
            Row(
              children: [
                Expanded(
                  child: statCard(
                    Icons.local_drink,
                    '$entries',
                    'Entries',
                  ),
                ),
                const SizedBox(width: 12),
                Expanded(
                  child: statCard(
                    Icons.water_drop,
                    '$water ml',
                    'Consumed',
                  ),
                ),
                const SizedBox(width: 12),
                Expanded(
                  child: statCard(
                    Icons.hourglass_bottom,
                    '$remaining ml',
                    'Remaining',
                  ),
                ),
              ],
            ),
            const SizedBox(height: 25),
            const Align(
              alignment: Alignment.centerLeft,
              child: Text(
                'Quick Add',
                style: TextStyle(
                  fontSize: 20,
                  fontWeight: FontWeight.bold,
                  color: Color(0xff123B63),
                ),
              ),
            ),
            const SizedBox(height: 15),
            Row(
              children: [
                Expanded(
                  child: waterButton(
                    '250 ml',
                    Icons.local_drink,
                    250,
                  ),
                ),
                const SizedBox(width: 12),
                Expanded(
                  child: waterButton(
                    '500 ml',
                    Icons.water_drop,
                    500,
                  ),
                ),
              ],
            ),
            const SizedBox(height: 12),
            Row(
              children: [
                Expanded(
                  child: waterButton(
                    '750 ml',
                    Icons.opacity,
                    750,
                  ),
                ),
                const SizedBox(width: 12),
                Expanded(
                  child: waterButton(
                    'Custom',
                    Icons.add,
                    null,
                  ),
                ),
              ],
            ),
            const SizedBox(height: 25),
            Container(
              width: double.infinity,
              padding: const EdgeInsets.all(18),
              decoration: BoxDecoration(
                color: Colors.white,
                borderRadius: BorderRadius.circular(18),
                border: Border.all(color: Colors.blue.shade100),
              ),
              child: const Row(
                children: [
                  Icon(Icons.lightbulb, color: Colors.orange, size: 30),
                  SizedBox(width: 15),
                  Expanded(
                    child: Text(
                      'Tip: Keep a water bottle nearby to remember to drink water regularly.',
                      style: TextStyle(
                        fontSize: 14,
                        color: Color(0xff455A64),
                      ),
                    ),
                  ),
                ],
              ),
            ),
            const SizedBox(height: 20),
            SizedBox(
              width: double.infinity,
              height: 50,
              child: OutlinedButton.icon(
                onPressed: showResetDialog,
                icon: const Icon(Icons.refresh),
                label: const Text(
                  'Reset Today',
                  style: TextStyle(fontSize: 16),
                ),
                style: OutlinedButton.styleFrom(
                  foregroundColor: Colors.red,
                  side: const BorderSide(color: Colors.red),
                  shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(15),
                  ),
                ),
              ),
            ),
            const SizedBox(height: 20),
          ],
        ),
      ),
    );
  }
}

class AboutPage extends StatelessWidget {
  const AboutPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('About HydroTrack')),
      body: ListView(
        padding: const EdgeInsets.all(25),
        children: const [
          SizedBox(height: 15),
          Icon(Icons.water_drop, size: 80, color: Colors.blue),
          SizedBox(height: 20),
          Text(
            'HydroTrack',
            textAlign: TextAlign.center,
            style: TextStyle(
              fontSize: 28,
              fontWeight: FontWeight.bold,
              color: Color(0xff123B63),
            ),
          ),
          SizedBox(height: 15),
          Text(
            'HydroTrack helps you record your daily water intake and monitor your progress toward your personal hydration goal.',
            textAlign: TextAlign.center,
            style: TextStyle(fontSize: 16, color: Colors.grey),
          ),
          SizedBox(height: 30),
          ListTile(
            leading: Icon(Icons.flag, color: Colors.blue),
            title: Text('Personal Goal'),
            subtitle: Text('Set and modify your daily water goal'),
          ),
          ListTile(
            leading: Icon(Icons.storage, color: Colors.blue),
            title: Text('Local Storage'),
            subtitle: Text('Your progress is saved on your device'),
          ),
          ListTile(
            leading: Icon(Icons.analytics, color: Colors.blue),
            title: Text('Progress Tracking'),
            subtitle: Text('Monitor consumed and remaining water'),
          ),
          ListTile(
            leading: Icon(Icons.favorite, color: Colors.red),
            title: Text('Healthy Habit'),
            subtitle: Text('Build a consistent hydration routine'),
          ),
        ],
      ),
    );
  }
}
```

## OUTPUT

<img width="936" height="595" alt="image" src="https://github.com/user-attachments/assets/f4ba4207-adc9-4c3a-99f7-53ee8a62d9e7" />

<img width="926" height="597" alt="image" src="https://github.com/user-attachments/assets/76346fb4-a760-4482-8c04-5560bdb6afe1" />

<img width="515" height="370" alt="image" src="https://github.com/user-attachments/assets/597fe5f6-4bcf-42c7-beb1-6edb4baa9618" />
