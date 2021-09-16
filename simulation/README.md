## Activity Simulations and Codes

## Activity-1 In Action

|OFF|OFF|OFF|ON|
|:--:|:--:|:--:|:--:|
|![OFF](https://user-images.githubusercontent.com/80662569/116460814-1e747f00-a885-11eb-9361-7d70ba90e82d.PNG) |![OFF](https://user-images.githubusercontent.com/80662569/116460808-1c122500-a885-11eb-8023-4ab0ec876fa6.PNG)|![OFF](https://user-images.githubusercontent.com/80662569/116460810-1ddbe880-a885-11eb-9460-5a43f89de00e.PNG)|![ON](https://user-images.githubusercontent.com/80662569/116460813-1ddbe880-a885-11eb-90f1-d0da5705cd19.PNG)|

## Activity 1 Code 
```
void peripheral_init(void)
{	
	DDRD |= (1<<PD2); // set PD2=1 for LED
    DDRD &= ~(1<<PD0); //clear bit
    PORTD |= (1<<PD0); //set bit PD0 for SeatSwitch
    DDRD &= ~(1<<PD1); //clear bit
    PORTD |= (1<<PD1); //set bit PD0 for HeaterSwitch
}

void TurnLED_ON(){
    LED_PORT |= (1<<LED_PIN); 
}

void TurnLED_OFF(){
    LED_PORT &= ~(1<<LED_PIN);
}

int act1=0;
int activity1_LED(void)
{
       peripheral_init();
        if(!(PIND&(1<<BUTTON_SENSOR )) && !(PIND&(1<<TEMP_SENSOR))) //both the switches are pressed
        { 
            act1=1;
        }
        else  //in all other cases
        {
            act1=0;
        }
    return act1;
}
  
```


## Activity-2 In Action

|ADC_00|ADC_11|
|:--:|:--:|
|![ADC_00](https://user-images.githubusercontent.com/80662569/116461383-d0ac4680-a885-11eb-84b5-dc8d17cff3dc.PNG) |![ADC_11](https://user-images.githubusercontent.com/80662569/116461375-cdb15600-a885-11eb-805c-2dc73d198a3d.PNG)|

## Activity 2 Code 
```
void InitADC()
{
    ADMUX=(1<<REFS0);
    ADCSRA= (1<<ADEN)|(7<<ADPS0);
}

uint16_t ReadADC(uint8_t ch)
{
    ADMUX&=0xf8;
    ch=ch&0b00000111;
    ADMUX|=ch;

    ADCSRA|=(1<<ADSC);
    while(!(ADCSRA & (1<<ADIF)));
    ADCSRA|=(1<<ADIF);
    return(ADC);
}

uint16_t activity2_GetADC(void)
{
      InitADC();
      uint16_t temp;
      temp=ReadADC(0);
      return temp;
}
  
```

## Activity-3 In Action

|PWM_20%|PWM_40%|
|:--:|:--:|
|![PWM_20%](https://user-images.githubusercontent.com/80662569/116461901-72cc2e80-a886-11eb-8525-42061d74f693.PNG) |![PWM_40%](https://user-images.githubusercontent.com/80662569/116461893-7069d480-a886-11eb-859b-1ec11c368164.PNG)|
|PWM_70%|PWM_95%|
|![PWM_70%](https://user-images.githubusercontent.com/80662569/116461897-72339800-a886-11eb-9006-369b7be40d44.PNG)|![PWM_95%](https://user-images.githubusercontent.com/80662569/116461900-72339800-a886-11eb-8eea-43aeae327bea.PNG)|

## Activity 3 Code 
```
void InitTimer()
{
    TCCR1A |= (1<<COM1A1)|(1<<WGM11)|(1<<WGM10);
    TCCR1B |= (1<<WGM12)|(1<<CS11)|(1<<CS10);
    DDRB |=(1<<PB1);
}

void activity3_PWM(uint16_t temp)
{
    InitTimer();
    if(temp>=0 && temp<=200){
            OCR1A = PWM_20_PERCENT;
            _delay_ms(200);
        }
        else if(temp>=210 && temp<=500){
             OCR1A = PWM_40_PERCENT;
            _delay_ms(200);
        }
        else if(temp>=510 && temp<=700){
             OCR1A = PWM_70_PERCENT;
            _delay_ms(200);
        }
        else if(temp>=710 && temp<=1024){
             OCR1A = PWM_95_PERCENT;
            _delay_ms(200);
        }
        else{
            OCR1A=0;
            _delay_ms(200);
        }

}

```

## Activity-4 In Action

|20 DEGREES|25 DEGREES|
|:--:|:--:|
|![20](https://user-images.githubusercontent.com/80662569/116462498-20d7d880-a887-11eb-827d-3c4601f65f82.PNG) |![25](https://user-images.githubusercontent.com/80662569/116462506-22a19c00-a887-11eb-9f96-e03d69035c7e.PNG)|
|29 DEGREES|33 DEGREES|
|![29](https://user-images.githubusercontent.com/80662569/116462509-233a3280-a887-11eb-9b8f-c925bc36cdcd.PNG)|![33](https://user-images.githubusercontent.com/80662569/116462511-233a3280-a887-11eb-91a9-a11098dc7529.PNG)|

## Activity 4 Code 
```
void USARTInit()
{
    UBRR0H = (uint8_t)(UBRR_VALUE>>8);
    UBRR0L = (uint8_t)UBRR_VALUE;
    UCSR0C=(1<<UMSEL00)|(1<<UCSZ01)|(1<<UCSZ00);
    UCSR0B=(1<<RXEN0)|(1<<TXEN0)|(1<<RXCIE0)|(1<<TXCIE0);
}

int USARTRead()
{
    while(!(UCSR0A &(1<<RXC0)))
    {
        //Do nothing
    }
    return UDR0;
}

void USARTInit()
{
    UBRR0H = (uint8_t)(UBRR_VALUE>>8);
    UBRR0L = (uint8_t)UBRR_VALUE;
    UCSR0C=(1<<UMSEL00)|(1<<UCSZ01)|(1<<UCSZ00);
    UCSR0B=(1<<RXEN0)|(1<<TXEN0)|(1<<RXCIE0)|(1<<TXCIE0);
}

int USARTRead()
{
    while(!(UCSR0A &(1<<RXC0)))
    {
        //Do nothing
    }
    return UDR0;
}

void activity4_USARTWrite(uint16_t temp)
{ 
    int i=0;
        if(temp>=0 && temp<=200){
            unsigned char data1[]="Temperature: 20 degree C\n";
            i=0;
            while(data1[i]!=0){
            while (!( UCSR0A & (1<<UDRE0)));  // Wait for empty transmit buffer
            UDR0 = data1[i];        // Put data into buffer, sends the data
            i++;
            }
            _delay_ms(2000);
        }
        else if(temp>=210 && temp<=500){
            unsigned char data2[]="Temperature: 25 degree C\n";
            i=0;
            while(data2[i]!=0){
            while (!( UCSR0A & (1<<UDRE0)));  // Wait for empty transmit buffer
            UDR0 = data2[i];        // Put data into buffer, sends the data
            i++;
            }
            _delay_ms(2000);
        }
        else if(temp>=510 && temp<=700){        
             unsigned char data3[]="Temperature: 29 degree C\n";
            i=0;
            while(data3[i]!=0){
            while (!( UCSR0A & (1<<UDRE0)));  // Wait for empty transmit buffer
            UDR0 = data3[i];        // Put data into buffer, sends the data
            i++;
            }
            _delay_ms(2000);
        }
        else if(temp>=710 && temp<=1024){         
            unsigned char data4[]="Temperature: 33 degree C\n";     
            i=0;
            while(data4[i]!=0){
            while (!( UCSR0A & (1<<UDRE0)));  // Wait for empty transmit buffer
            UDR0 = data4[i];        // Put data into buffer, sends the data
            i++;
            }
            _delay_ms(2000);
        }
        else{
              unsigned char data5[]="Temperature: 0 degree C\n";
            i=0;
            while(data5[i]!=0){
            while (!( UCSR0A & (1<<UDRE0)));  // Wait for empty transmit buffer
            UDR0 = data5[i];        // Put data into buffer, sends the data
            i++;
            }
            _delay_ms(2000);
            
        }
}
  
```
