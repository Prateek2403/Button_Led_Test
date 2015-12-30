#include <stdbool.h>
#include <stdint.h>
#include "inc/hw_memmap.h"
#include "inc/hw_types.h"
#include "inc/hw_gpio.h"
#include "driverlib/sysctl.h"
#include "driverlib/gpio.h"
#include "drivers/buttons.h"

uint32_t counter = 0, ret_button;
volatile uint32_t ui32Loop;

enum ButtonStates { UP, DOWN, PRESS, RELEASE };
 
enum ButtonStates delay_debounce(enum ButtonStates button_state) 
{        
	
    ret_button  = ButtonsPoll(0,0);

    if ((ret_button & ALL_BUTTONS) == LEFT_BUTTON)   /* if pressed     */
    {                      
        if (button_state == PRESS)
        {
            button_state = DOWN;
        } 
        if (button_state == UP)
         {
            SysCtlDelay(500);
            if ((ret_button & ALL_BUTTONS) == LEFT_BUTTON)
            {
                button_state = PRESS;
            }
        } 
    } 
    else 
    {                                 
        if (button_state == RELEASE)  /* if not pressed */
        {
            button_state = UP;
        } 
        if (button_state == DOWN)
        {
            if (!((ret_button & ALL_BUTTONS) == LEFT_BUTTON))
            {
                SysCtlDelay(500); 

                if (!((ret_button & ALL_BUTTONS) == LEFT_BUTTON))
                {
                    button_state = RELEASE;
                }
            }
        }
    }
    return button_state;
}

int main ()
{

	enum ButtonStates Bs;

    SysCtlPeripheralEnable(SYSCTL_PERIPH_GPIOF);

    //
    // Check if the peripheral access is enabled.
    //
    while(!SysCtlPeripheralReady(SYSCTL_PERIPH_GPIOF))
    {
    }

    //
    // Enable the GPIO pin for the LED (PF3).  Set the direction as output, and
    // enable the GPIO pin for digital function.
    //
	GPIOPinTypeGPIOOutput(GPIO_PORTF_BASE, GPIO_PIN_1);
	GPIOPinTypeGPIOOutput(GPIO_PORTF_BASE, GPIO_PIN_2);
	GPIOPinTypeGPIOOutput(GPIO_PORTF_BASE, GPIO_PIN_3);
    	
	ButtonsInit();

    //
    // Loop forever.
    //
    while(1)
    {

	ret_button  = ButtonsPoll(0,0);

	Bs = delay_debounce( Bs );

	if ( Bs == PRESS )
	{
	
		if (counter >= 3)
		{
			counter = 0;
		}
		
		ret_button  = ButtonsPoll(0,0);

		switch(counter)
		{		
		  case	0:	GPIOPinWrite(GPIO_PORTF_BASE, GPIO_PIN_2, 0x0);
				GPIOPinWrite(GPIO_PORTF_BASE, GPIO_PIN_3, 0x0);
				GPIOPinWrite(GPIO_PORTF_BASE, GPIO_PIN_1, GPIO_PIN_1); //RED
				counter += 1;
				break;
		  case	1: 	GPIOPinWrite(GPIO_PORTF_BASE, GPIO_PIN_1, 0x0);
				GPIOPinWrite(GPIO_PORTF_BASE, GPIO_PIN_3, 0x0);
				GPIOPinWrite(GPIO_PORTF_BASE, GPIO_PIN_2, GPIO_PIN_2); //BLUE
				counter += 1;
				break;
		  case	2:	GPIOPinWrite(GPIO_PORTF_BASE, GPIO_PIN_2, 0x0);
				GPIOPinWrite(GPIO_PORTF_BASE, GPIO_PIN_1, 0x0);
				GPIOPinWrite(GPIO_PORTF_BASE, GPIO_PIN_3, GPIO_PIN_3); //GREEN
				counter += 1;
				break;
		}

		

	}

	if ( (ret_button & ALL_BUTTONS) == RIGHT_BUTTON)
	{
	
		//
		// Turn off the LED.
		//
		GPIOPinWrite(GPIO_PORTF_BASE, GPIO_PIN_1, 0x0);	
		GPIOPinWrite(GPIO_PORTF_BASE, GPIO_PIN_2, 0x0);
		GPIOPinWrite(GPIO_PORTF_BASE, GPIO_PIN_3, 0x0);

	}
    }

}
