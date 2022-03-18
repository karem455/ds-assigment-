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
	Stack() { top = -1; }
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
		int i = 0;
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
	string infixToPostFix(string infix) {
		StackType<char> operators;
		bool isMathOperatorRepeated = false;
		bool isOperaendRepeated = false;
		string postfix;
		for (int i = 0; i < infix.size(); i++) {
			// Checking Operator
			if (infix[i] == '+' || infix[i] == '-' || infix[i] == '*' || infix[i] == '/') {
				if (isMathOperatorRepeated) {
					postfix = "Wrong Expression";

					/*
					After this for loop there is while loop
					which is checking rest of the char and add it with postfix string .
					So this pushed char should be pop out
					beacuse infix expression is wrong.
					*/

					while (!operators.IsEmpty())
					{
						operators.Pop();
					}
					break;
				}
				while (!operators.IsEmpty() && higherPrecedenceValidate(operators.Top(), infix[i]))
				{
					postfix = postfix + operators.Top();
					operators.Pop();
				}
				operators.Push(infix[i]);
				isMathOperatorRepeated = true;
				isOperaendRepeated = false;

			}
			// Checking Operand
			else if (infix[i] >= '0' && infix[i] <= '9')
			{
				if (isOperaendRepeated) {
					postfix = "Wrong Expression";

					/*
					After this for loop there is while loop
					which is checking rest of the char and add it with postfix string .
					So this pushed char should be pop out
					beacuse infix expression is wrong.
					*/

					while (!operators.IsEmpty())
					{
						operators.Pop();
					}
					break;
				}
				postfix = postfix + infix[i];
				isMathOperatorRepeated = false;
				isOperaendRepeated = true;

			}
			//Checking open bracket
			else if (infix[i] == '(') {
				operators.Push(infix[i]);
				isMathOperatorRepeated = false;
				isOperaendRepeated = false;
			}

			//Checking closing bracket
			else if (infix[i] == ')') {

				while (!operators.IsEmpty() && operators.Top() != '(')
				{
					postfix = postfix + operators.Top();
					operators.Pop();
				}

				/*
				checking stack beacuse we know
				that if the infix char is ')'
				and the stack is empty then the infix expression is wrong

				*/
				if (operators.IsEmpty()) {
					postfix = "Wrong Expression";
					break;

				}
				else {
					operators.Pop();
				}
				// poping the opening bracket
				isMathOperatorRepeated = false;
				isOperaendRepeated = false;
			}

			// checking that infix expression has invalid char
			else {
				postfix = "Wrong Expression";

				
				So this pushed char should be pop out
				beacuse infix expression is wrong.
				*/
				while (!operators.IsEmpty())
				{
					operators.Pop();
				}
				break;
			}


		};
};
int main() {
	Stack <char> p;
	int result;
	char infix[MAXSIZE];
	char result[MAXSIZE];
	cout << "enter expression" << endl;
	cin >> infix;
	if (infix != "Wrong Expression") {
		result = evaluatePostFix(postfix);
		cout << "Result: " << result << endl << endl;
		p.infixtopost(infix, postfix);

	}
		system("pause");


	}
