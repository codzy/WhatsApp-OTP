import 'package:flutter/material.dart';
import 'dart:math';
import 'package:url_launcher/url_launcher.dart';
import 'package:whatsapp_unilink/whatsapp_unilink.dart';

class WhatsappOtp extends StatefulWidget {
  @override
  State<WhatsappOtp> createState() => _WhatsappOtpState();
}

class _WhatsappOtpState extends State<WhatsappOtp> {
  final successsnackBar = SnackBar(content: Text('Correct'));
  final failedsnackBar = SnackBar(content: Text('Worng'));

  final GlobalKey<FormState> _formKey = GlobalKey();
  TextEditingController userOtp = TextEditingController();
  bool _isSent = false;
  int? _otp;
  var phoneNumber = '201010101010'; //CountryCode then number without + sign

  Future buttonPressed() async {
    _otp = sendOtp();
    await launch("https://wa.me/${phoneNumber}?text= your OTP is ${_otp}");
    setState(() {
      _isSent = true;
    });
  }

  void checkOtp() {
    if (userOtp.text == _otp.toString()) {
      ScaffoldMessenger.of(context).showSnackBar(successsnackBar);
    } else {
      ScaffoldMessenger.of(context).showSnackBar(failedsnackBar);
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          crossAxisAlignment: CrossAxisAlignment.center,
          children: [
            Container(
              margin: EdgeInsets.all(15),
              child: _isSent == false
                  ? Text("Press Send Otp")
                  : Text("Enter Sent Otp"),
            ),
            Form(
                key: _formKey,
                child: TextField(
                  controller: userOtp,
                  keyboardType: TextInputType.number,
                  maxLength: 5,
                  decoration: InputDecoration(
                      border: OutlineInputBorder(), enabled: _isSent),
                )),
            ElevatedButton(
                onPressed: () {
                  _isSent == false ? buttonPressed() : checkOtp();
                },
                child:
                    _isSent == false ? Text("Send Otp") : Text("Confirm Otp"))
          ],
        ),
      ),
    );
  }
}

int? sendOtp() {
  Random randomizer = Random();
  int min = 10000, max = 99999;
  int generatedOtp = min + randomizer.nextInt(max - min);
  print(generatedOtp);

  return generatedOtp;
}
