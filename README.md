# ArduinoCarMobile

#define in1 5  // Pin for IN1 on L298n
#define in2 6  // Pin for IN2 on L298n
#define in3 12  // Pin for IN1 on L298n
#define in4 13  // Pin for IN2 on L298n

#define enA 9 
#define enB 11 
char command; // Variable to store the incoming command
int pwmValue = 0; 

void setup() {
  pinMode(in1, OUTPUT);  // Set IN1 as an output
  pinMode(in2, OUTPUT);  // Set IN2 as an output
  pinMode(in3, OUTPUT);  // Set IN1 as an output
  pinMode(in4, OUTPUT);  // Set IN2 as an output
  pinMode(enA, OUTPUT); 
  pinMode(enB, OUTPUT); 
  
  Serial.begin(9600);  // Start serial communication at 9600 baud rate
}





void loop() {





  if (Serial.available() > 0) {
    command = Serial.read();  // Read the incoming byte
    



  if (command == 'F') {  
      // If the command is 'B' (Backward)
      digitalWrite(in1, LOW);
      digitalWrite(in2, HIGH); // If the command is 'B' (Backward)
      digitalWrite(in3, LOW);
      digitalWrite(in4, HIGH);
    }else if (command == 'B') {  

      digitalWrite(in1, HIGH);
      digitalWrite(in2, LOW);
      digitalWrite(in3, HIGH);
      digitalWrite(in4, LOW);
    }else if (command == 'L') {  

      digitalWrite(in1, LOW);
      digitalWrite(in2, HIGH);
      digitalWrite(in3, HIGH);
      digitalWrite(in4, LOW);
    }else if (command == 'R') {  

      digitalWrite(in1, HIGH);
      digitalWrite(in2, LOW);
      digitalWrite(in3, LOW);
      digitalWrite(in4, HIGH);
    }else if (command == 'S') {  // If the command is 'S' (Stop)
      digitalWrite(in1, LOW);
      digitalWrite(in2, LOW);// If the command is 'S' (Stop)
      digitalWrite(in3, LOW);
      digitalWrite(in4, LOW);
    } 
    
    
    
    else if (command == 'V') { // Set Voltage
      String voltageString = ""; // Initialize an empty string to store the voltage value
      delay(10); // Short delay to allow more characters to arrive
      while (Serial.available() > 0) {
        char nextChar = Serial.read();
        if (nextChar >= '0' && nextChar <= '9') {
          voltageString += nextChar; // Append numeric characters to the string
        } else {
          break; // Exit loop if non-numeric character received
        }
        delay(10); // Short delay to allow more characters to arrive
      }
      pwmValue = voltageString.toInt();  // Convert the string to an integer
      if (pwmValue > 255) {
        pwmValue = 255; // Ensure the PWM value does not exceed 255
      }
      analogWrite(enA, pwmValue);   
      analogWrite(enB, pwmValue);     
    }



    
  }
}
