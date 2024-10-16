Acest cod implementează jocul "Mastermind", un joc de ghicire a culorilor. În joc, utilizatorul trebuie să ghicească un cod secret format dintr-o secvență de culori. Codul este generat aleatoriu și utilizatorul are un număr limitat de încercări pentru a ghici secvența corectă.

Variabile globale:

COLORS: O listă care conține culorile valide pentru codul secret: ["R", "G", "B", "Y", "W", "O"]. Fiecare literă reprezintă o culoare (de ex., R = roșu, G = verde, B = albastru etc.).
TRIES: Un număr întreg care specifică numărul maxim de încercări pe care jucătorul le are pentru a ghici codul (în acest caz, 10 încercări).
CODE_LENGTH: Un număr întreg care specifică lungimea codului secret (în acest caz, 4 culori).

Funcții:

generate_code()
Această funcție generează un cod secret aleatoriu din culorile disponibile.

Descriere:

Creează o listă goală code.
Alege aleatoriu câte o culoare din lista COLORS și o adaugă în cod până când lungimea codului este egală cu CODE_LENGTH.
Returnează codul generat, care este o listă de culori.
Exemplu de output: ["R", "G", "B", "Y"]

def generate_code():
    code = []
    for _ in range(CODE_LENGHT):
        color = random.choice(COLORS)
        code.append(color)
    return code


guess_code()
Această funcție solicită utilizatorului să introducă o presupunere (o secvență de culori) și validează inputul.

Descriere:

Solicită utilizatorului să introducă culorile separate prin spații.
Converteste inputul în majuscule și împarte stringul într-o listă de culori.
Verifică dacă inputul are exact CODE_LENGTH culori.
Verifică dacă fiecare culoare introdusă este validă (există în lista COLORS).
Dacă inputul nu este valid, solicită utilizatorului să încerce din nou.
Returnează presupunerea corectă.



def guess_code():
    while True:
        guess = input("Guess:").upper().split(" ")
        if len(guess) != CODE_LENGHT:
            print(f"You must guess {CODE_LENGHT} colors.")
            continue

        for color in guess:
            if color not in COLORS:
                print(f"Invalid color: {color}. Try again")
                break
        else:
            break
    return guess


check_code(guess, real_code)
Această funcție verifică dacă presupunerea utilizatorului este corectă comparând-o cu codul secret.

Descriere:

Creează un dicționar color_counts pentru a număra aparițiile fiecărei culori în codul secret.
Inițializează două variabile pentru a urmări câte culori sunt pe poziția corectă (correct_pos) și câte culori sunt corecte, dar pe poziția greșită (incorrect_pos).
Parcurge presupunerea și codul real simultan pentru a număra câte culori sunt în poziția corectă.
Parcurge din nou presupunerea și codul pentru a număra culorile care sunt corecte, dar pe poziții greșite (fără a recunoaște de două ori aceeași culoare).
Returnează două valori: numărul de culori pe poziția corectă și numărul de culori corecte, dar pe poziții greșite.
Exemplu de output: (2, 1) – două culori pe poziția corectă și o culoare corectă, dar pe poziția greșită.


def check_code(guess, real_code):
    color_counts = {}
    correct_pos = 0
    incorrect_pos = 0

    for color in real_code:
        if color not in color_counts:
            color_counts[color] = 0
        color_counts[color] += 1

    for guess_color, real_color in zip(guess, real_code):
        if guess_color == real_color:
            correct_pos += 1
            color_counts[guess_color] -= 1

    for guess_color, real_color in zip(guess, real_code):
        if guess_color in color_counts and color_counts[guess_color] > 0:
            incorrect_pos += 1
            color_counts[guess_color] -= 1
    
    return correct_pos, incorrect_pos


game()
Aceasta este funcția principală care rulează jocul "Mastermind".

Descriere:

Afișează un mesaj de bun venit și explică regulile de bază.
Apelează funcția generate_code() pentru a genera codul secret.
Permite utilizatorului să încerce să ghicească codul folosind funcția guess_code().
Verifică fiecare presupunere utilizând check_code() și afișează numărul de poziții corecte și de culori corecte, dar pe poziții greșite.
Dacă utilizatorul ghicește codul corect, afișează un mesaj de victorie și încheie jocul.
Dacă utilizatorul nu ghicește codul în numărul maxim de încercări, afișează codul secret la final.



def game():
    print(f"Welcome to mastermind, You have {TRIES} to guess the code...")
    print("The valid colors are", *COLORS)

    code = generate_code()
    for attemps in range(1, TRIES + 1):
        guess = guess_code()
        correct_pos, incorrect_pos = check_code(guess, code)

        if correct_pos == CODE_LENGHT:
            print(f"You guessed the code in {attemps} tries!")
            break

        print(f"Correct Positions: {correct_pos} | Incorrect Positions: {incorrect_pos}")

    else:
        print("You run out of tries, the code was:", *code)


Observații:
Jocul oferă 10 încercări pentru a ghici codul secret.
Codul secret constă dintr-o secvență de 4 culori care pot fi duplicate.
După fiecare presupunere, utilizatorul este informat despre numărul de culori corect plasate și numărul de culori corecte, dar plasate greșit.

Mod de rulare:
Codul poate fi rulat direct într-un terminal sau IDE Python.
Utilizatorul va fi ghidat să introducă presupuneri până la rezolvarea codului sau până la epuizarea încercărilor.



