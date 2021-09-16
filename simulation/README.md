## Activity Simulations and Codes

## Activity-1 In Action

|OFF|OFF|ON|OFF|
|:--:|:--:|:--:|:--:|
|![OFF](https://user-images.githubusercontent.com/80662569/116460814-1e747f00-a885-11eb-9361-7d70ba90e82d.PNG) |![OFF](https://user-images.githubusercontent.com/80662569/116460808-1c122500-a885-11eb-8023-4ab0ec876fa6.PNG)|![ON](https://user-images.githubusercontent.com/80662569/116460813-1ddbe880-a885-11eb-90f1-d0da5705cd19.PNG)|![OFF](https://user-images.githubusercontent.com/80662569/116460810-1ddbe880-a885-11eb-9460-5a43f89de00e.PNG)

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

|PWM_20%|PWM_40%|PWM_70%|
|:--:|:--:|:--:|
|![PWM_20%](https://user-images.githubusercontent.com/80662569/116461901-72cc2e80-a886-11eb-8525-42061d74f693.PNG) |![PWM_40%](https://user-images.githubusercontent.com/80662569/116461893-7069d480-a886-11eb-859b-1ec11c368164.PNG)|![PWM_70%](https://user-images.githubusercontent.com/80662569/116461897-72339800-a886-11eb-9006-369b7be40d44.PNG)|
|PWM_95%|
|![PWM_95%](https://user-images.githubusercontent.com/80662569/116461900-72339800-a886-11eb-8eea-43aeae327bea.PNG)|

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

