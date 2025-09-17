from tkinter import *

root = Tk()
root.title("Gene Calculator")
root.geometry("500x400")

# ---------------- Function Section ---------------- #
def bald_cal(mom, dad, result_label):
    if dad == 1 and mom == 1:
        result_label.config(text="Boy: 100%\nGirl: 50%")
    elif dad == 1 and mom == 2:
        result_label.config(text="Boy: 75%\nGirl: 25%")
    elif dad == 2 and mom == 1:
        result_label.config(text="Boy: 100%\nGirl: 0%")
    elif dad == 2 and mom == 2:
        result_label.config(text="Boy: 50%\nGirl: 0%")
    else:
        result_label.config(text="Please select mom & dad genes.")

def thalassemia_cal(mom, dad, result_label):
    if dad == 1 and mom == 1:
        result_label.config(text="Boy: 100%\nGirl: 100%")
    elif dad == 1 and mom == 2:
        result_label.config(text="Boy: 50%\nGirl: 50%")
    elif dad == 2 and mom == 1:
        result_label.config(text="Boy: 50%\nGirl: 50%")
    elif dad == 2 and mom == 2:
        result_label.config(text="Boy: 25%\nGirl: 25%")
    else:
        result_label.config(text="Please select mom & dad genes.")

def g6pd_cal(mom, dad, result_label):
    if dad == 1 and mom == 1:
        result_label.config(text="Boy: 100%\nGirl: 100%")
    elif dad == 1 and mom == 2:
        result_label.config(text="Boy: 50%\nGirl: 50%")
    elif dad == 2 and mom == 1:
        result_label.config(text="Boy: 100%\nGirl: 0%")
    elif dad == 2 and mom == 2:
        result_label.config(text="Boy: 50%\nGirl: 0%")
    else:
        result_label.config(text="Please select mom & dad genes.")

# ---------------- Page Creation Function ---------------- #
def create_page(name, mom_options, dad_options, calc_func):
    frame = Frame(root)

    Label(frame, text=name, font=("Arial", 16)).pack(pady=10)

    mom_var = IntVar()
    dad_var = IntVar()

    # Left: Mom Options
    mom_frame = Frame(frame)
    mom_frame.pack(side="left", padx=20, pady=10)
    Label(mom_frame, text="Mother").pack()
    for text, value in mom_options:
        Radiobutton(mom_frame, text=text, variable=mom_var, value=value).pack(anchor="w")

    # Right: Dad Options
    dad_frame = Frame(frame)
    dad_frame.pack(side="right", padx=20, pady=10)
    Label(dad_frame, text="Father").pack()
    for text, value in dad_options:
        Radiobutton(dad_frame, text=text, variable=dad_var, value=value).pack(anchor="w")

    # Result Label
    result_label = Label(frame, text="Result will appear here.", fg="blue")
    result_label.pack(pady=20)

    # Calculate Button
    Button(frame, text="Calculate", command=lambda: calc_func(mom_var.get(), dad_var.get(), result_label)).pack()

    return frame

# ---------------- Pages Setup ---------------- #

# Page 1: Baldness
bald_page = create_page(
    "Baldness Gene",
    mom_options=[("Bald (aa)", 1), ("No Bald (Aa)", 2)],
    dad_options=[("Bald (Aa)", 1), ("No Bald (AA)", 2)],
    calc_func=bald_cal
)

# Page 2: Thalassemia
thal_page = create_page(
    "Thalassemia Gene",
    mom_options=[("Thalassemia (aa)", 1), ("Carrier (Aa)", 2)],
    dad_options=[("Thalassemia (aa)", 1), ("Carrier (Aa)", 2)],
    calc_func=thalassemia_cal
)

# Page 3: G6PD
g6pd_page = create_page(
    "G6PD Gene",
    mom_options=[("G6PD (xᵃxᵃ)", 1), ("Carrier (xᴬxᵃ)", 2)],
    dad_options=[("G6PD (xᵃy)", 1), ("Normal (xᴬy)", 2)],
    calc_func=g6pd_cal
)

# Page manager
pages = {
    "Bald%": bald_page,
    "Thalassemia": thal_page,
    "G6PD%": g6pd_page,
}

def show_page(page_name):
    for p in pages.values():
        p.pack_forget()
    pages[page_name].pack(fill="both", expand=True)

# Menu
menu_bar = Menu(root)
root.config(menu=menu_bar)

gene_menu = Menu(menu_bar, tearoff=0)
menu_bar.add_cascade(label="Genes", menu=gene_menu)
gene_menu.add_command(label="Bald%", command=lambda: show_page("Bald%"))
gene_menu.add_command(label="Thalassemia", command=lambda: show_page("Thalassemia"))
gene_menu.add_command(label="G6PD%", command=lambda: show_page("G6PD%"))

# Show first page by default
show_page("Bald%")

root.mainloop()
