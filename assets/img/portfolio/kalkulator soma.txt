import tkinter as tk
from tkinter import ttk

class Calculator:
    def __init__(self, root):
        self.root = root
        self.root.title("Kalkulator")
        self.root.geometry("400x600")

        self.equation = tk.StringVar()
        self.display = ttk.Entry(self.root, textvariable=self.equation, font=("Arial", 24), justify='right', state='readonly')
        self.display.grid(row=0, column=0, columnspan=4, ipadx=8, ipady=15, pady=20)

        self.create_buttons()

    def create_buttons(self):
        buttons = [
            '7', '8', '9', '/', 'C',
            '4', '5', '6', '*', '(',
            '1', '2', '3', '-', ')',
            '0', '.', '=', '+'
        ]

        row = 1
        col = 0

        for button in buttons:
            if button == "=":
                ttk.Button(self.root, text=button, command=self.calculate, width=10).grid(row=row, column=col, columnspan=2, ipadx=10, ipady=20)
                col += 2
            elif button == "C":
                ttk.Button(self.root, text=button, command=self.clear, width=10).grid(row=row, column=col, columnspan=1, ipadx=10, ipady=20)
                col += 1
            else:
                ttk.Button(self.root, text=button, command=lambda b=button: self.add_to_expression(b), width=10).grid(row=row, column=col, columnspan=1, ipadx=10, ipady=20)
                col += 1

            if col > 3:
                col = 0
                row += 1

    def add_to_expression(self, value):
        current_text = self.equation.get()
        new_text = current_text + str(value)
        self.equation.set(new_text)

    def clear(self):
        self.equation.set("")

    def calculate(self):
        try:
            result = eval(self.equation.get())
            self.equation.set(result)
        except Exception as e:
            self.equation.set("Error")

if __name__ == "__main__":
    root = tk.Tk()
    calculator = Calculator(root)
    root.mainloop()
