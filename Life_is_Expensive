import streamlit as st
import pandas as pd
import datetime
import os

# --- CONFIGURATION ---
FILE_NAME = "expenses.csv"
CATEGORIES = ["Food", "Transport", "Rent/Bills", "Leisure", "Shopping", "Other"]

st.set_page_config(page_title="Smart Expense Tracker", layout="centered")

# --- DATA LOADING ---
def load_data():
    if os.path.exists(FILE_NAME):
        return pd.read_csv(FILE_NAME)
    return pd.DataFrame(columns=["Date", "Category", "Amount", "Description"])

def save_data(df):
    df.to_csv(FILE_NAME, index=False)

# Initialize Data
data = load_data()

# --- UI ---
st.title("💸 Smart Expense Tracker")
st.markdown("Track your spending habits efficiently and simply.")

# Sidebar for Input
with st.sidebar:
    st.header("Add New Expense")
    date = st.date_input("Date", datetime.date.today())
    category = st.selectbox("Category", CATEGORIES)
    amount = st.number_input("Amount", min_value=0.0, step=0.01)
    desc = st.text_input("Description")
    
    if st.button("Add Expense"):
        new_entry = pd.DataFrame([[date, category, amount, desc]], 
                                 columns=["Date", "Category", "Amount", "Description"])
        data = pd.concat([data, new_entry], ignore_index=True)
        save_data(data)
        st.success("Expense added!")

# Main Dashboard
col1, col2 = st.columns(2)

with col1:
    total = data['Amount'].sum()
    st.metric("Total Spent", f"₪{total:,.2f}")

with col2:
    if not data.empty:
        most_expensive = data.groupby('Category')['Amount'].sum().idxmax()
        st.metric("Top Category", most_expensive)

# Visualizations
if not data.empty:
    st.subheader("Spending Breakdown")
    chart_data = data.groupby('Category')['Amount'].sum()
    st.bar_chart(chart_data)
    
    st.subheader("Recent Transactions")
    st.dataframe(data.sort_values(by="Date", ascending=False), use_container_width=True)
else:
    st.info("Start by adding an expense in the sidebar!")
