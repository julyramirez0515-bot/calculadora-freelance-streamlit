# calculadora-freelance-streamlit
import streamlit as st

# Configuración de la página
st.set_page_config(
    page_title="Calculadora Freelance Pro",
    page_icon="💼",
    layout="centered"
)

# Estilo simple personalizado
st.markdown("""
    <style>
        .main {
            background-color: #f4f6f9;
        }
        .stButton>button {
            background-color: #0066ff;
            color: white;
            border-radius: 8px;
            height: 3em;
            width: 100%;
        }
        .stButton>button:hover {
            background-color: #0047b3;
        }
    </style>
""", unsafe_allow_html=True)

# Título
st.title("💼 Calculadora de Presupuesto Freelance")
st.write("Calcula cuánto deberías cobrar como freelancer de manera profesional.")

st.divider()

# -------------------------
# DATOS DE ENTRADA
# -------------------------

st.subheader("📊 Datos Mensuales")

gastos = st.number_input("Gastos mensuales ($)", min_value=0.0, value=0.0)
horas_mes = st.number_input("Horas trabajadas al mes", min_value=1.0, value=160.0)

st.subheader("⚙️ Configuración")

margen = st.slider("Margen de ganancia (%)", 0, 100, 30)
impuestos = st.slider("Impuestos (%)", 0, 100, 0)
ahorro = st.slider("Ahorro / Fondo de emergencia (%)", 0, 100, 10)

st.divider()

# -------------------------
# CÁLCULO TARIFA
# -------------------------

if st.button("🚀 Calcular tarifa por hora"):

    if horas_mes <= 0:
        st.error("Las horas trabajadas deben ser mayores a 0")
    else:
        costo_base = gastos / horas_mes
        tarifa = costo_base * (1 + margen / 100)
        tarifa *= (1 + impuestos / 100)
        tarifa *= (1 + ahorro / 100)

        st.session_state["tarifa"] = tarifa

        st.success(f"💰 Tarifa recomendada por hora: ${tarifa:,.2f}")

# -------------------------
# CÁLCULO PROYECTO
# -------------------------

if "tarifa" in st.session_state:
    st.divider()
    st.subheader("📦 Calculadora por Proyecto")

    horas_proyecto = st.number_input(
        "Horas estimadas del proyecto",
        min_value=1.0,
        value=10.0
    )

    if st.button("Calcular precio del proyecto"):
        total = st.session_state["tarifa"] * horas_proyecto
        st.info(f"📌 Precio recomendado del proyecto: ${total:,.2f}")

st.divider()

st.caption("Desarrollado en Python con Streamlit 🚀")
