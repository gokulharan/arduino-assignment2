# arduino-assignment2
#include <avr/io.h>
#include <avr/interrupt.h>

#define lm35Pin A0      // LM35 temperature sensor pin
#define ledPin 13       // Onboard LED pin

volatile uint8_t ledState = 0;

void setup() {
  pinMode(ledPin, OUTPUT);

  // Configure Timer1 for interrupt every 250 milliseconds
  TCCR1A = 0;
  TCCR1B = 0;
  TCNT1 = 0;
  OCR1A = 62499;  // 16MHz / (prescaler * desired frequency) - 1
  TCCR1B |= (1 << WGM12);  // CTC mode
  TCCR1B |= (1 << CS12) | (1 << CS10);  // 1024 prescaler
  TIMSK1 |= (1 << OCIE1A);  // Enable Timer1 Compare A interrupt

  sei();  // Enable global interrupts
}

void loop() {
  int temperature = readTemperature();

  if (temperature < 30) {
     
  } else {
    
    digitalWrite(ledPin, LOW);
  }
}

ISR(TIMER1_COMPA_vect) {
  // Timer1 Compare A interrupt routine
  ledState = !ledState;
  if (ledState) {
    PORTB |= (1 << PB5);  // Set PB5 (digital pin 13) high
  } else {
    PORTB &= ~(1 << PB5);  // Set PB5 (digital pin 13) low
  }
}

int readTemperature() {
  // Replace this function with your code to read the LM35 temperature sensor
  // and return the temperature value.
  // For simplicity, I'll return a dummy value of 25 here.
  return 25;
}
