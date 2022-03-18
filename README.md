#include <iostream>
#include<string>
#include<conio.h>
using namespace std;
template<class T>
#define MAXSIZE 100
class Stack {
	int top;
	T items[MAXSIZE];
public:
	Stack() {top = -1; }
	int isfull() {
		if (top == MAXSIZE - 1)
			return 1;
		else
			return 0;
	}
	int isempty() {
		if (top == -1)
			return 1;
		else
			return 0;
	}
	void push(T x) {
		if (isfull()) {
			cout << "overflow" << endl;
		}
		else {
			items[top++] = x;
		}
	}
	T pop() {
			if (isempty()) {
			cout << "underflow" << endl;
		}
		else
			return items[top--];
	}
	int properity(char symbol) {
		if (symbol == '(')
			return 1;
		else if (symbol == '+' || symbol == '-')
			return 2;
		else if (symbol == '*' || symbol == '/')
			return 3;
		else if (symbol == '^' || symbol == '%')
			return 4;
		else 
			return 0;
	}
	int isoperator(char s) {
		if (s == '+' || s == '-' || s == '/' || s == '*' || s == '%' || s == '^')
			return 1;
		else
			return 0;
	}
	void infixtopost(char inf[], char post[]) {
		int len;
		len = strlen(inf);
		inf[len++] = ')';
		int i=0;
		char item;
		int j = 0;
		char x;
		push('(');
		item = inf[i];
		while (item != '\0') {
			
				if (item == '(') {
					push(item);
				}
				else if (isdigit(item) || isalpha(item)) {
					post[j] = item;
					j++;
				}
				else if (isoperator(item) == 1) {
					x = pop();
					while (isoperator(x == 1) && properity(item) <= properity(x)) {
						post[j] = x;
						j++;
						x = pop();
					}
					push(x);
					push(item);
				}
				else if (item == ')') {
					x = pop();
					while (item != '(') {
						post[j] = x;
						j++;
						x = pop();
					}

				}
				else {
					cout << " invalid expression" << endl;
				}
				i++;
		}
	}

};
int main() {
	Stack <char> p;
	char in[MAXSIZE];
	char post[MAXSIZE];
	cout << "enter expression" << endl;
	cin >> in;
	p.infixtopost(in, post);
	cout << post << endl;
	return 0;
}
