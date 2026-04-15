Banco de dados de uma escola (sqlite3)
import sqlite3

# ===== BANCO =====
conexao = sqlite3.connect('escola.db')
cursor = conexao.cursor()

cursor.execute("""
CREATE TABLE IF NOT EXISTS alunos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nome TEXT,
    sala TEXT,
    nota1 REAL,
    nota2 REAL,
    nota3 REAL,
    faltas INTEGER,
    ocorridos TEXT,
    pagamento TEXT
)
""")

conexao.commit()


# ===== FUNÇÕES =====

def criar_aluno():
    nome = input("Nome do aluno: ")
    sala = input("Sala: ")

    cursor.execute(
        "INSERT INTO alunos (nome, sala) VALUES (?, ?)",
        (nome, sala)
    )
    conexao.commit()
    print("Aluno criado!")


def adicionar_nota():
    nome = input("Nome do aluno: ")
    nota = float(input("Nota: "))

    cursor.execute(
        "UPDATE alunos SET nota1=? WHERE nome=?",
        (nota, nome)
    )
    conexao.commit()
    print("Nota salva!")


def adicionar_falta():
    nome = input("Nome do aluno: ")
    faltas = int(input("Faltas: "))

    cursor.execute(
        "UPDATE alunos SET faltas=? WHERE nome=?",
        (faltas, nome)
    )
    conexao.commit()
    print("Faltas registradas!")


def adicionar_ocorridos():
    nome = input("Nome do aluno: ")
    ocorridos = input("Ocorrência: ")

    cursor.execute(
        "UPDATE alunos SET ocorridos=? WHERE nome=?",
        (ocorridos, nome)
    )
    conexao.commit()
    print("Ocorrência registrada!")


def ver_salas():
    cursor.execute("SELECT DISTINCT sala FROM alunos")
    salas = cursor.fetchall()

    for s in salas:
        print("Sala:", s[0])


def alunos_por_sala():
    sala = input("Digite a sala: ")

    cursor.execute(
        "SELECT nome FROM alunos WHERE sala=?",
        (sala,)
    )

    alunos = cursor.fetchall()

    for a in alunos:
        print(a[0])


def registrar_pagamento():
    nome = input("Nome do aluno: ")
    status = input("Pago ou não pago: ")

    cursor.execute(
        "UPDATE alunos SET pagamento=? WHERE nome=?",
        (status, nome)
    )
    conexao.commit()
    print("Pagamento atualizado!")


def painel_aluno():
    nome = input("Nome do aluno: ")

    cursor.execute(
        "SELECT * FROM alunos WHERE nome=?",
        (nome,)
    )

    aluno = cursor.fetchone()

    if aluno:
        print("\n--- PORTAL DO ALUNO ---")
        print("Nome:", aluno[1])
        print("Sala:", aluno[2])
        print("Nota 1:", aluno[3])
        print("Nota 2:", aluno[4])
        print("Nota 3:", aluno[5])
        print("Faltas:", aluno[6])
        print("Ocorridos:", aluno[7])
        print("Pagamento:", aluno[8])
    else:
        print("Aluno não encontrado!")


# ===== SISTEMA =====

tipo = input('Escolha o painel aluno, professor, diretor: ').lower()

if tipo == "professor":
    print('Entrando no painel do professor')

    while True:
        print("1 - adicionar nota")
        print("2 - adicionar falta")
        print("3 - ocorrências")
        print("0 - sair")

        op = input("Escolha: ")

        if op == "1":
            adicionar_nota()

        elif op == "2":
            adicionar_falta()

        elif op == "3":
            adicionar_ocorridos()

        elif op == "0":
            break

        else:
            print("Opção inválida")


elif tipo == "diretor":
    print('Entrando no painel do diretor')

    while True:
        print("1 - criar aluno")
        print("2 - ver salas")
        print("3 - alunos por sala")
        print("4 - pagamento")
        print("0 - sair")

        op = input("Escolha: ")

        if op == "1":
            criar_aluno()

        elif op == "2":
            ver_salas()

        elif op == "3":
            alunos_por_sala()

        elif op == "4":
            registrar_pagamento()

        elif op == "0":
            break

        else:
            print("Opção inválida")


elif tipo == "aluno":
    print('Entrando no modo aluno')
    painel_aluno()

else:
    print("Opção inválida")
