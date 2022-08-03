# 3D-Donut-Math
Making a 3D rotatng donut using &lt;C> language 

```c
//Donut Math
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <conio.h>
#include <math.h>

void render_frame(const int screen_width, const int screen_height, int buffer_size, float z_buffer[buffer_size], char output_buffer[buffer_size], float A, float B)
{
	//Torus making angles
	float theta, phi;
	//Spacing Variables
	const float theta_step = 0.02;
	const float phi_step = 0.02;
	//Circle radius and constant
	const float R¹ = 3;
	const float R² = 1.8;
	const float K¹ = 10;
	//X = R¹+ R²;
	//We want our torus to be displaced 1/8th of the width of the screen,
	//which is 1/4th of the way from the center to the side of the screen.
	const int K = (screen_width * K¹ * 1) / (8 * (R¹ + R²));

	memset(output_buffer, ' ', buffer_size);
	memset(z_buffer, 0, buffer_size * sizeof(float));

	for (theta = 0; theta <= 6.2831; theta += theta_step)
	{
		for (phi = 0; phi <= 6.2831; phi += phi_step)
		{
			float sin_phi = sin(phi),
				  cos_phi = cos(phi),
				  sin_theta = sin(theta),
				  cos_theta = cos(theta),
				  sinA = sin(A),
				  cosA = cos(A),
				  sinB = sin(B),
				  cosB = cos(B),
				  circlex = R¹ + (R² * cos_phi),
				  circley = (R² * sin_phi);
			//3D co-ordinate after rotation
			float X = (circlex * cos_theta * cosB + circlex * sin_theta * sinA * sinB + circley * sinB * cosA);
			float Y = (-circlex * sinB * cos_theta + circlex * sin_theta * sinA * cosB + circley * cosA * cosB);
			float Z = (circlex * sin_theta * cosA - circley * sinA);
			//pixel depth info
			float Zi = 1 / (Z + K¹);
			//Xp and Yp or 3D projection co-ordinates
			int Xp = screen_width / 2 + X * 2 * K * Zi;
			int Yp = (screen_height / 2 + 1) - Y * K * Zi;
			int i = Xp + screen_width * Yp;

			//Calculating illuminance, in the range of 12 different levels (from 0 to 11) (8*sqrt(2) = 11.3)
			float L = (-cos_phi * sinB * cos_theta + cos_phi * sin_theta * sinA * cosB + sin_phi * cosA * cosB - cos_phi * sin_theta * cosA + sin_phi * sinA);
			int illuminance_lvl = L * 8;

			if (Xp > 0 && Xp < screen_width && Yp > 0 && Yp < screen_height && Zi > z_buffer[i])
			{
				z_buffer[i] = Zi;
				output_buffer[i] = ".,-~:;=!*#$@"[illuminance_lvl > 0 ? illuminance_lvl : 0];
			}
		}
	}
}
void display(int buffer_size, const int screen_width, char output_buffer[buffer_size])
{
	//Putting the cursor to home position
	gotoxy(0, 0);

	for (int k = 0; k <= buffer_size + 1; k++)
	{
		putchar(k % screen_width ? output_buffer[k] : '\n');
	}
}
int main()
{
	//Screen Resolution Variables
	const int screen_width = 85;
	const int screen_height = 22;
	//Buffer variables
	int buffer_size = 1870;
	float z_buffer[buffer_size];
	char output_buffer[buffer_size];
	//X and Z axis rotation angles
	float A = 0, B = 0;
	//Putting the cursor to home position
	gotoxy(0, 0);
	//Clearing the screen
	system("cls");

	for (;;)
	{
		//Function Calling
		//Generating the frame
		render_frame(screen_width, screen_height, buffer_size, z_buffer, output_buffer, A, B);
		//Displaying the frame
		display(buffer_size, screen_width, output_buffer);
		//Spacing Variables for X and Z axis rotation
		A += 0.03;
		B += 0.03;
	}
}
```
