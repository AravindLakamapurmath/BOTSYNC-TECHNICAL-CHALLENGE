#BOTSYNC-TECHNICAL-CHALLENGE

#include "stm32f10x.h"
#define NUM_SWITCHES 4
#define SWITCH_PINS {GPIO_Pin_34, GPIO_Pin_33, GPIO_Pin_32, GPIO_Pin_31}
#define LED_PINS {GPIO_Pin_10, GPIO_Pin_11, GPIO_Pin_12, GPIO_Pin_13}
GPIO_InitTypeDef GPIO_InitStructure;
int switchStates[NUM_SWITCHES] = {0};
void GPIO_Configuration(void) {
  RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_InitStructure.GPIO_Pin = SWITCH_PINS;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;
  GPIO_Init(GPIOA, &GPIO_InitStructure);
  GPIO_InitStructure.GPIO_Pin = LED_PINS;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
  GPIO_Init(GPIOA, &GPIO_InitStructure);
}
int main(void) {
  GPIO_Configuration();
  while (1) {
    // read switch states and turn on/off corresponding LEDs
    for (int i = 0; i < NUM_SWITCHES; i++) {
      switchStates[i] = GPIO_ReadInputDataBit(GPIOA, SWITCH_PINS[i]);
      if (switchStates[i] == GPIO_Pin_Reset) {
        GPIO_SetBits(GPIOA, LED_PINS[i]);
      } else {
        GPIO_ResetBits(GPIOA, LED_PINS[i]);
      }
    }
  }
}
