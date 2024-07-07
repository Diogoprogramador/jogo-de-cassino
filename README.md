# jogo-de-cassino
import tkinter as tk
import random


class CasinoGame:
    def __init__(self, root):
        self.root = root
        self.root.title("Jogo de Cassino - Frutas e Diamantes")

        self.initial_balance = 1000
        self.balance = self.initial_balance
        self.bet_amount = 50
        self.symbols = ['🍒', '🍋', '🍇', '💎', '🍊']
        self.result = [random.choice(self.symbols), random.choice(self.symbols), random.choice(self.symbols)]

        self.label_balance = tk.Label(root, text=f"Saldo: ${self.balance}", font=('Arial', 18))
        self.label_balance.pack(pady=20)

        self.label_result = tk.Label(root, text=f"{' '.join(self.result)}", font=('Arial', 50))
        self.label_result.pack(pady=20)

        self.label_message = tk.Label(root, text="Faça sua aposta e clique em 'Girar'", font=('Arial', 14))
        self.label_message.pack(pady=10)

        self.entry_bet = tk.Entry(root, width=10, font=('Arial', 16))
        self.entry_bet.pack(pady=10)

        self.spin_button = tk.Button(root, text="Girar", font=('Arial', 16), command=self.spin)
        self.spin_button.pack(pady=20)

        self.quit_button = tk.Button(root, text="Sair", font=('Arial', 16), command=root.quit)
        self.quit_button.pack(pady=10)

    def spin(self):
        try:
            self.bet_amount = int(self.entry_bet.get())
            if self.bet_amount <= 0:
                raise ValueError
        except ValueError:
            self.label_message.config(text="Por favor, insira uma aposta válida (número inteiro maior que zero).")
            return

        if self.bet_amount > self.balance:
            self.label_message.config(text=f"Você não tem saldo suficiente. Saldo atual: ${self.balance}.")
            return

        self.balance -= self.bet_amount
        self.result = [random.choice(self.symbols), random.choice(self.symbols), random.choice(self.symbols)]
        self.label_result.config(text=f"{' '.join(self.result)}")

        if self.result.count('💎') == 3:
            winnings = self.bet_amount * 10
            self.balance += winnings
            self.label_message.config(text=f"💎💎💎 Jackpot! Você ganhou ${winnings}!")
        elif self.result.count('💎') == 2:
            winnings = self.bet_amount * 5
            self.balance += winnings
            self.label_message.config(text=f"💎💎 Você ganhou ${winnings}!")
        elif self.result.count('💎') == 1:
            winnings = self.bet_amount * 2
            self.balance += winnings
            self.label_message.config(text=f"💎 Você ganhou ${winnings}!")
        elif self.result.count('🍒') == 3:
            winnings = self.bet_amount * 3
            self.balance += winnings
            self.label_message.config(text=f"🍒🍒🍒 Três cerejas! Você ganhou ${winnings}!")
        elif self.result.count('🍒') == 2:
            winnings = self.bet_amount * 2
            self.balance += winnings
            self.label_message.config(text=f"🍒🍒 Duas cerejas! Você ganhou ${winnings}!")
        elif self.result.count('🍒') == 1:
            winnings = self.bet_amount
            self.balance += winnings
            self.label_message.config(text=f"🍒 Uma cereja! Você ganhou ${winnings}.")
        else:
            self.label_message.config(text=f"Não foi dessa vez. Tente novamente!")

        self.label_balance.config(text=f"Saldo: ${self.balance}")
        if self.balance <= 0:
            self.label_message.config(text="Você ficou sem dinheiro! O jogo acabou.")


if __name__ == '__main__':
    root = tk.Tk()
    game = CasinoGame(root)
    root.mainloop()
