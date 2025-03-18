import pandas as pd

# Datos para la Lista de la Compra
data_lista = {
    "Producto": [
        "Yogur natural (2% grasa)", "Muesli", "Pan integral",
        "Frutos rojos congelados", "Almendras/nueces",
        "Tomate triturado (enlatado)", "Tomate fresco", "Tomate cherry",
        "Calabacín", "Zanahoria", "Pimiento", "Brócoli", "Espinacas",
        "Carne de vaca picada (5% grasa)", "Pollo", "Atún al natural (enlatado)",
        "Huevos", "Garbanzos cocidos (enlatado)", "Pasta integral",
        "Bulgur", "Arroz integral", "Aceite de oliva virgen extra (AOVE)"
    ],
    "Cantidad": [
        "600 g", "90 g", "1 paquete (400 g; usar ~200 g)",
        "240 g", "110 g", "600 g (2 latas)", "300 g", "200 g",
        "350 g", "300 g", "325 g", "500 g", "200 g",
        "1 kg", "500 g", "1 lata (160 g)", "6 unidades",
        "400 g (usar 300 g)", "500 g (usar 320 g)", "500 g (usar 280 g)",
        "500 g (usar 140 g)", "250 ml (usar ~140 ml)"
    ],
    "Precio Estimado (DKK)": [
        "25", "7", "25", "15", "22", "30", "10", "20", "15", "10",
        "20", "25", "15", "70", "45", "15", "20", "12", "20", "25",
        "20", "50"
    ]
}
df_lista = pd.DataFrame(data_lista)

# Datos para Macros por Comida
data_macros = {
    "DÍA": [
        "Lunes (Entreno)", "Martes (Entreno)", "Miércoles (Descanso - Low Carb)",
        "Jueves (Entreno)", "Viernes (Entreno)", "Sábado (Descanso - Low Carb)",
        "Domingo (Entreno)"
    ],
    "Desayuno": [
        "150g yogur, 30g muesli, 80g frutos rojos (350 kcal)",
        "80g pan, 50g aguacate, 60g queso cottage, tomate (350 kcal)",
        "150g yogur, 20g nueces (300 kcal)",
        "80g pan, 50g aguacate, 60g queso cottage, tomate (350 kcal)",
        "150g yogur, 30g muesli, 80g frutos rojos (350 kcal)",
        "40g pan, 50g aguacate, 1 huevo duro (300 kcal)",
        "150g yogur, 30g muesli, 80g frutos rojos (350 kcal)"
    ],
    "Comida": [
        "80g pasta seca, 120g vaca picada, 150g tomate triturado, 10g AOVE (600 kcal)",
        "70g bulgur, 120g pollo, 200g verduras, 10g AOVE (600 kcal)",
        "150g garbanzos, 80g atún, 1 huevo, 100g espinaca, 100g tomate cherry, 10g AOVE (500 kcal)",
        "120g vaca, 70g arroz, 250g verduras, 10g AOVE (600 kcal)",
        "80g pasta seca, 120g vaca picada, 150g tomate triturado, 10g AOVE (600 kcal)",
        "120g vaca, 250g verduras, 10g AOVE (500 kcal)",
        "70g bulgur, 120g pollo, 200g verduras, 10g AOVE (600 kcal)"
    ],
    "Cena": [
        "Repetir pasta boloñesa",
        "Repetir bulgur con pollo",
        "Repetir ensalada garbanzos",
        "Repetir carne con arroz y verduras",
        "Repetir pasta boloñesa",
        "Repetir carne con verduras",
        "Repetir bulgur con pollo"
    ],
    "Snack": [
        "15g frutos secos (100 kcal)",
        "15g frutos secos (100 kcal)",
        "15g frutos secos (120 kcal)",
        "15g frutos secos (100 kcal)",
        "15g frutos secos (100 kcal)",
        "20g frutos secos (130 kcal)",
        "15g frutos secos (100 kcal)"
    ],
    "Total Día": [
        "1800 kcal", "1850 kcal", "1800 kcal", "1850 kcal", "1800 kcal",
        "1750-1800 kcal", "1850 kcal"
    ]
}
df_macros = pd.DataFrame(data_macros)

# Generar archivo Excel con dos hojas
with pd.ExcelWriter("PlanSemanal.xlsx") as writer:
    df_lista.to_excel(writer, sheet_name="Lista de la compra", index=False)
with pd.ExcelWriter("PlanSemanal.xlsx", engine="openpyxl", mode="a") as writer:
    df_macros.to_excel(writer, sheet_name="Macros por comida", index=False)

print("Archivo Excel generado: PlanSemanal.xlsx")

# Generar PDF con la información usando fpdf
from fpdf import FPDF

class PDF(FPDF):
    def header(self):
        self.set_font("Arial", "B", 14)
        self.cell(0, 10, "Plan Semanal y Lista de la Compra", ln=True, align="C")
        self.ln(5)
    
    def chapter_title(self, title):
        self.set_font("Arial", "B", 12)
        self.cell(0, 10, title, ln=True)
        self.ln(3)
    
    def chapter_body(self, body):
        self.set_font("Arial", "", 10)
        self.multi_cell(0, 7, body)
        self.ln()

pdf = PDF()
pdf.add_page()
pdf.chapter_title("Lista de la Compra")
body_lista = ""
for i in range(len(df_lista)):
    row = df_lista.iloc[i]
    body_lista += f"{row['Producto']}: {row['Cantidad']} - {row['Precio Estimado (DKK)']} DKK\n"
pdf.chapter_body(body_lista)

pdf.add_page()
pdf.chapter_title("Macros por Comida")
body_macros = ""
for i in range(len(df_macros)):
    row = df_macros.iloc[i]
    body_macros += (
        f"{row['DÍA']}:\n"
        f"  Desayuno: {row['Desayuno']}\n"
        f"  Comida: {row['Comida']}\n"
        f"  Cena: {row['Cena']}\n"
        f"  Snack: {row['Snack']}\n"
        f"  Total: {row['Total Día']}\n\n"
    )
pdf.chapter_body(body_macros)

pdf.output("PlanSemanal.pdf")
print("Archivo PDF generado: PlanSemanal.pdf")
