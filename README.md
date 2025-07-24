import 'package:flutter/material.dart';
import 'dart:math';

void main() {
  runApp(AviatorPredictorApp());
}

class AviatorPredictorApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Aviator Predictor',
      home: AviatorHomePage(),
      debugShowCheckedModeBanner: false,
    );
  }
}

class AviatorHomePage extends StatefulWidget {
  @override
  _AviatorHomePageState createState() => _AviatorHomePageState();
}

class _AviatorHomePageState extends State<AviatorHomePage> {
  final TextEditingController _controller = TextEditingController();
  String prediction = '';

  void analyzeAndPredict() {
    try {
      List<double> values = _controller.text
          .split(',')
          .map((e) => double.parse(e.trim()))
          .toList();

      if (values.length < 10) {
        setState(() {
          prediction = '⚠️ অন্তত ১০টি রাউন্ড দিন';
        });
        return;
      }

      double avg = values.sublist(values.length - 10).reduce((a, b) => a + b) / 10;
      double last = values.last;

      String result;
      if (last < 1.5 && avg > 3.0) {
        result = '🔥 বড় জাম্পের সম্ভাবনা! সম্ভবত 4x+ আসবে';
      } else if (last < 1.5) {
        result = '⚠️ কম ক্র্যাশের সম্ভাবনা! সাবধানে থাকুন';
      } else {
        result = '🟢 2x পর্যন্ত যেতে পারে। Safe zone ধারণা';
      }

      setState(() {
        prediction = result;
      });
    } catch (e) {
      setState(() {
        prediction = '❌ ডেটা ঠিকভাবে লিখুন (যেমন: 1.45, 2.12, 3.10)';
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('🎯 Aviator Real-Time Predictor'),
        backgroundColor: Colors.deepPurple,
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            TextField(
              controller: _controller,
              maxLines: 5,
              decoration: InputDecoration(
                hintText: 'ক্র্যাশ ডেটা লিখুন (কমা দিয়ে আলাদা করুন)',
                border: OutlineInputBorder(),
              ),
              keyboardType: TextInputType.multiline,
            ),
            SizedBox(height: 16),
            ElevatedButton(
              onPressed: analyzeAndPredict,
              child: Text('🔍 প্রেডিকশন করুন'),
              style: ElevatedButton.styleFrom(backgroundColor: Colors.deepPurple),
            ),
            SizedBox(height: 24),
            Text(
              prediction,
              style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
            ),
          ],
        ),
      ),
    );
  }
}
