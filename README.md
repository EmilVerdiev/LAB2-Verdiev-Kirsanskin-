#define _CRT_SECURE_NO_WARNINGS 
#include <stdio.h> 
#include <string.h> 
#include <stdlib.h> 
#include <malloc.h> 
#include <math.h> 
#include <time.h> 

char one[252] = { '\0' }, str[6][20] = { '\0' };
int kol_slov = 0, kol_bukv = 0;

int proverka(char buk[2][11]) // перевод цифр-символов в число
{
	int sum = 0, i, n, itog = 0, p, j;

	for (j = 0; j < kol_bukv; j++)
		for (int j1 = 0; j1 < kol_bukv; j1++)
		{
			if (buk[1][j] == buk[1][j1] && j != j1)
				return 0;
		}

	for (j = 0; j < kol_slov + 1; j++)
	{
		n = strlen(str[j]) - 1;
		i = 0;

		while (str[j][i] != '\0')
		{
			p = 0;
			while (buk[0][p] != str[j][i])
			{
				p++;
			}

			if (i == 0 && buk[1][p] == '0')
				return 0;

			if (j != kol_slov)
				sum = sum + (buk[1][p] - '0') * pow(10, n);
			else
				itog = itog + (buk[1][p] - '0') * pow(10, n);

			n--;
			i++;
		}
	}

	if (sum == itog)
		return 1;
	else
		return 0;
}

void podbor(char buk[2][11], int p)
{
	int k = 0, n;

	while (p != 0)
	{
		if (p == kol_bukv)
		{
			for (int i = 0; i < 10; i++)
			{
				buk[1][p - 1] = i + '0';

				k = proverka(buk);
				if (k == 1)
				{
					n = 0;
					while (buk[0][n] != '\0')
					{
						printf("%c - %c", buk[0][n], buk[1][n]);
						printf("\n");
						n++;
					}
					return;
				}
			}
			buk[1][p - 1] = '0';
			p--;
		}
		else
		{
			if (buk[1][p - 1] == '9')
			{
				buk[1][p - 1] = '0';
				p--;
			}
			else
			{
				buk[1][p - 1]++;
				p++;
			}
		}
	}
}

int main()
{
	char buk[2][11] = { '\0' };
	int k = 0, i = 0, i2 = 0;
	char* c;
	scanf("%s", one);

	clock_t begin = clock();

	do
	{
		str[kol_slov][i2] = one[i];

		if (!strchr(buk[0], one[i]))
		{
			buk[0][k] = one[i];

			if (i2 == 0)
				buk[1][k] = '0';
			else
				buk[1][k] = '0';

			k++;
		}
		i++;
		i2++;

		if (one[i] == '+')
		{
			i++;
			kol_slov++;
			i2 = 0;
		}

		if (one[i] == '=')
		{
			i++;
			kol_slov++;
			i2 = 0;
		}

	} while (one[i] != '\0');


	kol_bukv = strlen(buk[1]);
	podbor(buk, kol_bukv);

	clock_t end = clock();
	double time_spent = (double)(end - begin) / CLOCKS_PER_SEC;

	printf("\n");
	printf("%.4lf", time_spent);

	return 0;
}

