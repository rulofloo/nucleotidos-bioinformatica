# nucleotidos-bioinformatica
import streamlit as st
from Bio.Seq import Seq
import random
import matplotlib.pyplot as plt

# Título del Dashboard
st.title("Análisis de Nucleótidos - Bioinformática")

# Menú lateral
option = st.sidebar.selectbox(
    "Selecciona una opción:",
    ["Análisis de Secuencia", "Simulación de Mutaciones", "Visualización de Resultados"]
)

# Función: Análisis de Secuencia
if option == "Análisis de Secuencia":
    st.header("Análisis de Secuencia")
    sequence = st.text_area("Introduce una secuencia de ADN:", "ATGCGTAGCTAG")
    if sequence:
        seq = Seq(sequence.upper())
        st.write(f"*Longitud de la Secuencia:* {len(seq)} nucleótidos")
        gc_content = (seq.count("G") + seq.count("C")) / len(seq) * 100
        st.write(f"*Contenido GC:* {gc_content:.2f}%")
        
        # Gráfica de distribución
        counts = {base: seq.count(base) for base in "ATGC"}
        fig, ax = plt.subplots()
        ax.bar(counts.keys(), counts.values(), color=['red', 'blue', 'green', 'purple'])
        ax.set_title("Distribución de Nucleótidos")
        ax.set_xlabel("Bases")
        ax.set_ylabel("Frecuencia")
        st.pyplot(fig)

# Función: Simulación de Mutaciones
elif option == "Simulación de Mutaciones":
    st.header("Simulación de Mutaciones")
    sequence = st.text_area("Introduce una secuencia de ADN para mutar:", "ATGCGTAGCTAG")
    mutation_count = st.slider("Cantidad de mutaciones:", 1, 10, 1)
    if sequence:
        seq = list(sequence.upper())
        for _ in range(mutation_count):
            idx = random.randint(0, len(seq) - 1)
            original_base = seq[idx]
            new_base = random.choice([b for b in "ATGC" if b != original_base])
            seq[idx] = new_base
        mutated_sequence = "".join(seq)
        st.write(f"*Secuencia Original:* {sequence}")
        st.write(f"*Secuencia Mutada:* {mutated_sequence}")

# Función: Visualización de Resultados
elif option == "Visualización de Resultados":
    st.header("Visualización de Resultados")
    st.write("Sube un archivo de datos genómicos en formato FASTA o TXT.")
    file = st.file_uploader("Cargar archivo", type=["fasta", "txt"])
    if file:
        sequence = file.read().decode("utf-8").strip()
        st.write("*Secuencia Cargada:*")
        st.text(sequence)
        seq = Seq(sequence.upper())
        counts = {base: seq.count(base) for base in "ATGC"}
        fig, ax = plt.subplots()
        ax.pie(counts.values(), labels=counts.keys(), autopct='%1.1f%%', colors=['red', 'blue', 'green', 'purple'])
        ax.set_title("Composición de Nucleótidos")
        st.pyplot(fig)
