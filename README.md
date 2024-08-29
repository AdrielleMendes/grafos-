from ssl import _PasswordType
from tkinter import Button, Entry, Label, Tk, messagebox
import sqlite3
import random

def banco():
    conexao = sqlite3.connect('usuarios.db')
    cursor = conexao.cursor()
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS clientes (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            email TEXT NOT NULL,
            senha TEXT NOT NULL
        )
    ''')
    conexao.commit()
    conexao.close()

def verificar_usuario(email, senha):
    conexao = sqlite3.connect('usuarios.db')
    cursor = conexao.cursor()
    cursor.execute('SELECT * FROM clientes WHERE email = ? AND senha = ?', (email, senha))
    usuario = cursor.fetchone()
    conexao.close()
    return usuario

def login():
    email = email.get()
    senha = senha.get()
 
    if not email or not senha:
        messagebox.showwarning("Campos obrigatórios", "Por favor, preencha todos os campos.")
        return

    usuario = verificar_usuario(email, senha)
    if usuario:
        messagebox.showinfo("Login", "Usuário autenticado com sucesso!")
    else:
        messagebox.showerror("Erro", "Usuário não encontrado ou senha incorreta.")

def appLogin():
    tela = Tk()
    tela.title("Login")
    tela.geometry("350x350")

    texto2 = Label(tela, text="Tela de Login", font=('Helvetica', 18, 'bold'))
    texto2.pack(padx=3, pady=3)
    
    txt_email = Label(tela, text="E-MAIL", font=('Helvetica', 16))
    txt_email.pack(padx=5, pady=5)
    campo_email = Entry(tela, width=35)
    campo_email.pack(padx=5, pady=5)

    txt_senha = Label(tela, text="SENHA", font=('Helvetica', 16))
    txt_senha.pack(padx=5, pady=5)
    campo_senha = Entry(tela, width=35, show="*")
    campo_senha.pack(padx=5, pady=5)

    



tela = Tk()
tela.title("Recuperar Senha")
tela.geometry("350x250")
txt_email = Label(tela, text="E-MAIL", font=('Helvetica', 16))
txt_email.pack(padx=10, pady=5)
campo_email = Entry(tela, width=35)
campo_email.pack(padx=10, pady=5)

txt_codigo = Label(tela, text="CÓDIGO DE VERIFICAÇÃO", font=('Helvetica', 16))
txt_codigo.pack(padx=10, pady=5)
campo_codigo = Entry(tela, width=35)
campo_codigo.pack(padx=10, pady=5)

txt_nova_senha = Label(tela, text="NOVA SENHA", font=('Helvetica', 16))
txt_nova_senha.pack(padx=10, pady=5)
campo_nova_senha = Entry(tela, width=35, show="*")
campo_nova_senha.pack(padx=10, pady=5)

txt_confirma_senha = Label(tela, text="CONFIRMAR SENHA", font=('Helvetica', 16))
txt_confirma_senha.pack(padx=10, pady=5)
campo_confirma_senha = Entry(tela, width=35, show="*")
campo_confirma_senha.pack(padx=10, pady=5)

def enviar_codigo():
    email = campo_email.get()
    user = email(email)
    
    if user:
        codigo = str(random.randint(100000, 999999))
        
        print(f"Código de verificação para {email}: {codigo}")
        
        tela.codigo_recuperacao = codigo
        tela.email_recuperacao = email
        messagebox.showinfo("Código Enviado", "Código de verificação enviado. Verifique seu e-mail.")
    else:
        messagebox.showerror("Erro", "Email não encontrado.")
    
def atualizar_senha():
    codigo_digitado = campo_codigo.get()
    nova_senha = campo_nova_senha.get()
    confirmar_senha = campo_confirma_senha.get()
    
    if codigo_digitado == getattr(tela, 'codigo_recuperacao', ''):
        if nova_senha == confirmar_senha:
            _PasswordType(tela.email_recuperacao, nova_senha)
            messagebox.showinfo("Senha Atualizada", "Senha atualizada com sucesso!")
            tela.destroy()
        else:
            messagebox.showerror("Erro", "A nova senha e a confirmação não coincidem.")
    else:
        messagebox.showerror("Erro", "Código de verificação inválido.")
    
botao_enviar_codigo = Button(tela, text="Enviar Código", command=enviar_codigo, font=('Helvetica', 14))
botao_enviar_codigo.pack(padx=10, pady=10)

botao_atualizar_senha = Button(tela, text="Atualizar Senha", command=atualizar_senha, font=('Helvetica', 14))
botao_atualizar_senha.pack(padx=10, pady=10)

tela.mainloop()



banco()
appLogin()


