import streamlit as st
import pandas as pd
import folium
from streamlit_folium import st_folium

st.set_page_config(page_title="Calculadora de Paradas", layout="wide")

nodos = pd.DataFrame([
    {"id": "A", "nombre": "Terminal Central", "lat": 14.9555, "lon": -91.7912},
    {"id": "B", "nombre": "Parada 1", "lat": 14.9827, "lon": -91.8031},
    {"id": "C", "nombre": "Parada 2", "lat": 15.0123, "lon": -91.8210},
    {"id": "D", "nombre": "Parada 3", "lat": 15.0471, "lon": -91.8385},
    {"id": "E", "nombre": "Universidad", "lat": 15.0692, "lon": -91.9038},
])
orden = list(nodos["id"])

st.title("Calculadora de Puntos de Parada en Rutas Predefinidas")

col1, col2 = st.columns(2)
with col1:
    origen_id = st.selectbox("Origen", orden, index=0)
with col2:
    destino_id = st.selectbox("Destino", orden, index=len(orden)-1)

i, j = orden.index(origen_id), orden.index(destino_id)
if i == j:
    st.warning("El origen y el destino no pueden ser iguales.")
else:
    ini, fin = (i, j) if i < j else (j, i)
    intermedias = orden[ini+1:fin]
    total = len(intermedias)

    o_nom = nodos.loc[nodos.id==origen_id, "nombre"].iloc[0]
    d_nom = nodos.loc[nodos.id==destino_id, "nombre"].iloc[0]
    st.success(f"Entre **{o_nom}** y **{d_nom}** hay **{total}** parada(s) intermedia(s).")

    center = [nodos["lat"].mean(), nodos["lon"].mean()]
    m = folium.Map(location=center, zoom_start=11, tiles="OpenStreetMap")

    for _, r in nodos.iterrows():
        color = "green" if r["id"]==origen_id else "red" if r["id"]==destino_id else "blue"
        folium.Marker(
            [r["lat"], r["lon"]],
            popup=f'{r["id"]} - {r["nombre"]}',
            tooltip=r["nombre"],
            icon=folium.Icon(color=color)
        ).add_to(m)

    coords_ruta = nodos.set_index("id").loc[orden][["lat","lon"]].values.tolist()
    folium.PolyLine(coords_ruta, weight=4, opacity=0.6).add_to(m)

    tramo_ids = orden[ini:fin+1]
    coords_tramo = nodos.set_index("id").loc[tramo_ids][["lat","lon"]].values.tolist()
    folium.PolyLine(coords_tramo, weight=6, opacity=0.9, color="red").add_to(m)

    st_folium(m, height=560, width=None)
