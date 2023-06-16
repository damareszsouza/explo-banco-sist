# explo-banco-sist
Exemplo de sistema bancário 


import pymysql

def connect():
    return pymysql.connect(host='localhost',
                           user='root',
                           password='',
                           db='banco')

def saque(conta, valor):
    conn = connect()
    cursor = conn.cursor()
    cursor.execute(f"SELECT saldo FROM contas WHERE numero = {conta}")
    saldo = cursor.fetchone()[0]
    if saldo >= valor:
        cursor.execute(f"UPDATE contas SET saldo = {saldo - valor} WHERE numero = {conta}")
        conn.commit()
        print("Saque realizado com sucesso!")
    else:
        print("Saldo insuficiente.")
    conn.close()

def deposito(conta, valor):
    conn = connect()
    cursor = conn.cursor()
    cursor.execute(f"SELECT saldo FROM contas WHERE numero = {conta}")
    saldo = cursor.fetchone()[0]
    cursor.execute(f"UPDATE contas SET saldo = {saldo + valor} WHERE numero = {conta}")
    conn.commit()
    print("Depósito realizado com sucesso!")
    conn.close()

def transferencia(conta_origem, conta_destino, valor):
    conn = connect()
    cursor = conn.cursor()
    cursor.execute(f"SELECT saldo FROM contas WHERE numero = {conta_origem}")
    saldo_origem = cursor.fetchone()[0]
    if saldo_origem >= valor:
        cursor.execute(f"SELECT saldo FROM contas WHERE numero = {conta_destino}")
        saldo_destino = cursor.fetchone()[0]
        cursor.execute(f"UPDATE contas SET saldo = {saldo_origem - valor} WHERE numero = {conta_origem}")
        cursor.execute(f"UPDATE contas SET saldo = {saldo_destino + valor} WHERE numero = {conta_destino}")
        conn.commit()
        print("Transferência realizada com sucesso!")
    else:
        print("Saldo insuficiente.")
    conn.close()


