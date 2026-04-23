import tkinter as tk
from tkinter import ttk, messagebox
import psycopg2


COR_FUNDO        = "#F8F0FC"
COR_ROXO         = "#4A0080"
COR_ROXO_CLARO   = "#7B2FBE"
COR_BRANCO       = "#FFFFFF"
COR_TEXTO        = "#2D2D2D"
COR_DESTAQUE     = "#E8D5F5"
COR_BOTAO_HOVER  = "#6A1FA0"

FONTE_TITULO  = ("Helvetica", 20, "bold")
FONTE_SUBTIT  = ("Helvetica", 13, "bold")
FONTE_NORMAL  = ("Helvetica", 11)
FONTE_BOTAO   = ("Helvetica", 11, "bold")
FONTE_SMALL   = ("Helvetica", 9)


def conectar():
    try:
        return psycopg2.connect(
            host="localhost",
            database="Clinica.petsvet",
            user="postgres",
            password="140912",
        )
    except Exception as e:
        messagebox.showerror("Erro de Conexão", str(e))
        return None


def botao_roxo(parent, texto, comando, largura=22):
    btn = tk.Button(
        parent, text=texto, command=comando,
        bg=COR_ROXO, fg=COR_BRANCO,
        font=FONTE_BOTAO, relief="flat",
        cursor="hand2", width=largura, pady=8,
        activebackground=COR_BOTAO_HOVER,
        activeforeground=COR_BRANCO
    )
    return btn

def campo_entry(parent, placeholder=""):
    frame = tk.Frame(parent, bg=COR_BRANCO, bd=1, relief="solid")
    entry = tk.Entry(
        frame, font=FONTE_NORMAL,
        bg=COR_BRANCO, fg=COR_TEXTO,
        relief="flat", bd=6,
        insertbackground=COR_ROXO
    )
    entry.pack(fill="x")
    if placeholder:
        entry.insert(0, placeholder)
        entry.config(fg="grey")
        def on_focus_in(e):
            if entry.get() == placeholder:
                entry.delete(0, tk.END)
                entry.config(fg=COR_TEXTO)
        def on_focus_out(e):
            if entry.get() == "":
                entry.insert(0, placeholder)
                entry.config(fg="grey")
        entry.bind("<FocusIn>",  on_focus_in)
        entry.bind("<FocusOut>", on_focus_out)
    return frame, entry

def label_titulo(parent, texto):
    tk.Label(
        parent, text=texto,
        font=FONTE_TITULO, bg=COR_FUNDO,
        fg=COR_ROXO
    ).pack(pady=(18, 4))

def separador(parent):
    tk.Frame(parent, bg=COR_ROXO_CLARO, height=2).pack(fill="x", padx=30, pady=6)


def tela_login(root):
    win = tk.Toplevel(root)
    win.title("Login — Clínica Vet")
    win.geometry("380x480")
    win.configure(bg=COR_FUNDO)
    win.resizable(False, False)
    win.grab_set()

    # Header
    header = tk.Frame(win, bg=COR_ROXO, height=120)
    header.pack(fill="x")
    tk.Label(header, text="", font=("Helvetica", 36),
             bg=COR_ROXO, fg=COR_BRANCO).pack(pady=(18, 0))
    tk.Label(header, text="Clínica Veterinária",
             font=FONTE_TITULO, bg=COR_ROXO, fg=COR_BRANCO).pack()

    body = tk.Frame(win, bg=COR_FUNDO)
    body.pack(fill="both", expand=True, padx=36, pady=24)

    tk.Label(body, text="Usuário", font=FONTE_SUBTIT,
             bg=COR_FUNDO, fg=COR_ROXO).pack(anchor="w", pady=(0, 4))
    f1, e_user = campo_entry(body, "admin")
    f1.pack(fill="x", pady=(0, 14))

    tk.Label(body, text="Senha", font=FONTE_SUBTIT,
             bg=COR_FUNDO, fg=COR_ROXO).pack(anchor="w", pady=(0, 4))
    f2, e_senha = campo_entry(body, "••••")
    e_senha.config(show="*")
    f2.pack(fill="x", pady=(0, 24))

    def fazer_login():
        u = e_user.get()
        s = e_senha.get()
        if u == "admin" and s == "123":
            win.destroy()
            tela_principal(root)
        else:
            messagebox.showerror("Erro", "Login ou senha inválidos!", parent=win)

    botao_roxo(body, "  Entrar", fazer_login).pack(fill="x")
    tk.Label(body, text="Save More • Care More ",
             font=FONTE_SMALL, bg=COR_FUNDO, fg=COR_ROXO_CLARO).pack(pady=16)


def tela_principal(root):
    win = tk.Toplevel(root)
    win.title("FOP Pet Shop — Clínica Vet")
    win.geometry("900x620")
    win.configure(bg=COR_FUNDO)
    win.resizable(False, False)

    # ── Sidebar ──────────────────────────
    sidebar = tk.Frame(win, bg=COR_ROXO, width=200)
    sidebar.pack(side="left", fill="y")
    sidebar.pack_propagate(False)

    tk.Label(sidebar, text=, font=("Helvetica", 32),
             bg=COR_ROXO, fg=COR_BRANCO).pack(pady=(28, 0))
    tk.Label(sidebar, text="FOP Pet Shop",
             font=("Helvetica", 13, "bold"),
             bg=COR_ROXO, fg=COR_BRANCO).pack(pady=(4, 24))

    conteudo = tk.Frame(win, bg=COR_FUNDO)
    conteudo.pack(side="right", fill="both", expand=True)

    def limpar():
        for w in conteudo.winfo_children():
            w.destroy()

    menus = [
        ("  Clientes",   lambda: secao_clientes(conteudo, limpar)),
        ("  Pets",       lambda: secao_pets(conteudo, limpar)),
        ("  Consultas",  lambda: secao_consultas(conteudo, limpar)),
    ]
    for txt, cmd in menus:
        btn = tk.Button(
            sidebar, text=txt, command=cmd,
            bg=COR_ROXO, fg=COR_BRANCO,
            font=FONTE_BOTAO, relief="flat",
            cursor="hand2", anchor="w",
            padx=20, pady=12,
            activebackground=COR_BOTAO_HOVER,
            activeforeground=COR_BRANCO
        )
        btn.pack(fill="x")

    # Dashboard inicial
    secao_dashboard(conteudo)


def secao_dashboard(parent):
    for w in parent.winfo_children():
        w.destroy()

    label_titulo(parent, " em-vindo ao FOP Pet Shop")
    separador(parent)

    cards_frame = tk.Frame(parent, bg=COR_FUNDO)
    cards_frame.pack(pady=24)

    def card(frame, emoji, titulo, subtitulo):
        c = tk.Frame(frame, bg=COR_BRANCO, bd=0,
                     relief="flat", padx=24, pady=20,
                     highlightbackground=COR_DESTAQUE,
                     highlightthickness=2)
        c.pack(side="left", padx=16)
        tk.Label(c, text=emoji, font=("Helvetica", 30),
                 bg=COR_BRANCO).pack()
        tk.Label(c, text=titulo, font=FONTE_SUBTIT,
                 bg=COR_BRANCO, fg=COR_ROXO).pack()
        tk.Label(c, text=subtitulo, font=FONTE_SMALL,
                 bg=COR_BRANCO, fg="grey").pack()

    card(cards_frame, "Clientes",  "Gerencie clientes")
    card(cards_frame, "Pets",      "Gerencie animais")
    card(cards_frame,  "Consultas", "Gerencie consultas")

    tk.Label(parent, text="Save More • Care More ",
             font=("Helvetica", 10), bg=COR_FUNDO,
             fg=COR_ROXO_CLARO).pack(pady=30)


def secao_clientes(parent, limpar):
    limpar()
    label_titulo(parent, "👤  Clientes")
    separador(parent)

    form = tk.Frame(parent, bg=COR_FUNDO)
    form.pack(pady=10, padx=40, fill="x")

    tk.Label(form, text="Nome", font=FONTE_NORMAL,
             bg=COR_FUNDO, fg=COR_TEXTO).grid(row=0, column=0, sticky="w", pady=4)
    f1, e_nome = campo_entry(form)
    f1.grid(row=0, column=1, padx=10, pady=4, sticky="ew")

    tk.Label(form, text="Email", font=FONTE_NORMAL,
             bg=COR_FUNDO, fg=COR_TEXTO).grid(row=1, column=0, sticky="w", pady=4)
    f2, e_email = campo_entry(form)
    f2.grid(row=1, column=1, padx=10, pady=4, sticky="ew")
    form.columnconfigure(1, weight=1)

    def cadastrar():
        conn = conectar()
        if not conn: return
        cur = conn.cursor()
        cur.execute("INSERT INTO clientes (nome, email) VALUES (%s, %s)",
                    (e_nome.get(), e_email.get()))
        conn.commit(); cur.close(); conn.close()
        messagebox.showinfo(" Sucesso", "Cliente cadastrado!")
        listar()

    def deletar():
        sel = tree.focus()
        if not sel:
            messagebox.showwarning("Aviso", "Selecione um cliente!")
            return
        id_c = tree.item(sel)["values"][0]
        if messagebox.askyesno("Confirmar", f"Deletar cliente ID {id_c}?"):
            conn = conectar()
            if not conn: return
            cur = conn.cursor()
            cur.execute("DELETE FROM consultas WHERE id_pet IN (SELECT id FROM pet WHERE id_cliente=%s)", (id_c,))
            cur.execute("DELETE FROM pet WHERE id_cliente=%s", (id_c,))
            cur.execute("DELETE FROM clientes WHERE id=%s", (id_c,))
            conn.commit(); cur.close(); conn.close()
            listar()

    def listar():
        for row in tree.get_children():
            tree.delete(row)
        conn = conectar()
        if not conn: return
        cur = conn.cursor()
        cur.execute("SELECT * FROM clientes ORDER BY nome")
        for r in cur.fetchall():
            tree.insert("", "end", values=r)
        cur.close(); conn.close()

    btn_frame = tk.Frame(parent, bg=COR_FUNDO)
    btn_frame.pack(pady=6)
    botao_roxo(btn_frame, "  Cadastrar", cadastrar, 18).pack(side="left", padx=6)
    botao_roxo(btn_frame, "  Deletar",  deletar,  18).pack(side="left", padx=6)
    botao_roxo(btn_frame, "  Atualizar Lista", listar, 18).pack(side="left", padx=6)

    cols = ("ID", "Nome", "Email")
    tree = ttk.Treeview(parent, columns=cols, show="headings", height=10)
    style = ttk.Style()
    style.theme_use("clam")
    style.configure("Treeview.Heading", background=COR_ROXO,
                    foreground=COR_BRANCO, font=FONTE_BOTAO)
    style.configure("Treeview", rowheight=28, font=FONTE_NORMAL)
    for c in cols:
        tree.heading(c, text=c)
        tree.column(c, anchor="center")
    tree.pack(padx=30, pady=10, fill="both", expand=True)
    listar()


def secao_pets(parent, limpar):
    limpar()
    label_titulo(parent, "  Pets")
    separador(parent)

    form = tk.Frame(parent, bg=COR_FUNDO)
    form.pack(pady=10, padx=40, fill="x")

    labels = ["Nome do Pet", "Espécie", "ID do Dono"]
    entries = []
    for i, l in enumerate(labels):
        tk.Label(form, text=l, font=FONTE_NORMAL,
                 bg=COR_FUNDO, fg=COR_TEXTO).grid(row=i, column=0, sticky="w", pady=4)
        f, e = campo_entry(form)
        f.grid(row=i, column=1, padx=10, pady=4, sticky="ew")
        entries.append(e)
    form.columnconfigure(1, weight=1)
    e_nome, e_esp, e_id = entries

    def cadastrar():
        conn = conectar()
        if not conn: return
        cur = conn.cursor()
        cur.execute("INSERT INTO pet (nome, especie, id_cliente) VALUES (%s,%s,%s)",
                    (e_nome.get(), e_esp.get(), e_id.get()))
        conn.commit(); cur.close(); conn.close()
        messagebox.showinfo(" Sucesso", "Pet cadastrado!")
        listar()

    def deletar():
        sel = tree.focus()
        if not sel:
            messagebox.showwarning("Aviso", "Selecione um pet!")
            return
        id_p = tree.item(sel)["values"][0]
        if messagebox.askyesno("Confirmar", f"Deletar pet ID {id_p}?"):
            conn = conectar()
            if not conn: return
            cur = conn.cursor()
            cur.execute("DELETE FROM consultas WHERE id_pet=%s", (id_p,))
            cur.execute("DELETE FROM pet WHERE id=%s", (id_p,))
            conn.commit(); cur.close(); conn.close()
            listar()

    def listar():
        for row in tree.get_children():
            tree.delete(row)
        conn = conectar()
        if not conn: return
        cur = conn.cursor()
        cur.execute("SELECT * FROM pet")
        for r in cur.fetchall():
            tree.insert("", "end", values=r)
        cur.close(); conn.close()

    btn_frame = tk.Frame(parent, bg=COR_FUNDO)
    btn_frame.pack(pady=6)
    botao_roxo(btn_frame, "  Cadastrar", cadastrar, 18).pack(side="left", padx=6)
    botao_roxo(btn_frame, "  Deletar",  deletar,  18).pack(side="left", padx=6)
    botao_roxo(btn_frame, "  Atualizar Lista", listar, 18).pack(side="left", padx=6)

    cols = ("ID", "Nome", "Espécie", "ID Cliente")
    tree = ttk.Treeview(parent, columns=cols, show="headings", height=10)
    for c in cols:
        tree.heading(c, text=c)
        tree.column(c, anchor="center")
    tree.pack(padx=30, pady=10, fill="both", expand=True)
    listar()


def secao_consultas(parent, limpar):
    limpar()
    label_titulo(parent, "🩺  Consultas")
    separador(parent)

    form = tk.Frame(parent, bg=COR_FUNDO)
    form.pack(pady=10, padx=40, fill="x")

    labels = ["Data (YYYY-MM-DD)", "Descrição", "ID do Pet"]
    entries = []
    for i, l in enumerate(labels):
        tk.Label(form, text=l, font=FONTE_NORMAL,
                 bg=COR_FUNDO, fg=COR_TEXTO).grid(row=i, column=0, sticky="w", pady=4)
        f, e = campo_entry(form)
        f.grid(row=i, column=1, padx=10, pady=4, sticky="ew")
        entries.append(e)
    form.columnconfigure(1, weight=1)
    e_data, e_desc, e_pet = entries

    def cadastrar():
        conn = conectar()
        if not conn: return
        cur = conn.cursor()
        cur.execute("INSERT INTO consultas (data, descricao, id_pet) VALUES (%s,%s,%s)",
                    (e_data.get(), e_desc.get(), e_pet.get()))
        conn.commit(); cur.close(); conn.close()
        messagebox.showinfo(" Sucesso", "Consulta cadastrada!")
        listar()

    def deletar():
        sel = tree.focus()
        if not sel:
            messagebox.showwarning("Aviso", "Selecione uma consulta!")
            return
        id_c = tree.item(sel)["values"][0]
        if messagebox.askyesno("Confirmar", f"Deletar consulta ID {id_c}?"):
            conn = conectar()
            if not conn: return
            cur = conn.cursor()
            cur.execute("DELETE FROM consultas WHERE id=%s", (id_c,))
            conn.commit(); cur.close(); conn.close()
            listar()

    def listar():
        for row in tree.get_children():
            tree.delete(row)
        conn = conectar()
        if not conn: return
        cur = conn.cursor()
        cur.execute("""
            SELECT consultas.id, clientes.nome, pet.nome,
                   consultas.data, consultas.descricao
            FROM consultas
            INNER JOIN pet      ON consultas.id_pet    = pet.id
            INNER JOIN clientes ON pet.id_cliente      = clientes.id
            ORDER BY consultas.data
        """)
        for r in cur.fetchall():
            tree.insert("", "end", values=r)
        cur.close(); conn.close()

    btn_frame = tk.Frame(parent, bg=COR_FUNDO)
    btn_frame.pack(pady=6)
    botao_roxo(btn_frame, "  Cadastrar", cadastrar, 18).pack(side="left", padx=6)
    botao_roxo(btn_frame, "  Deletar",  deletar,  18).pack(side="left", padx=6)
    botao_roxo(btn_frame, "  Atualizar Lista", listar, 18).pack(side="left", padx=6)

    cols = ("ID", "Cliente", "Pet", "Data", "Descrição")
    tree = ttk.Treeview(parent, columns=cols, show="headings", height=10)
    for c in cols:
        tree.heading(c, text=c)
        tree.column(c, anchor="center")
    tree.pack(padx=30, pady=10, fill="both", expand=True)
    listar()


if __name__ == "__main__":
    root = tk.Tk()
    root.withdraw()
    tela_login(root)
    root.mainloop()

