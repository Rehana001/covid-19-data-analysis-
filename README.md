# covid-19-data-analysis-

from os import name
import pandas as pd
import streamlit as st
import plotly.express as px
import plotly.offline as py
from plotly import tools
import plotly.figure_factory as ff
from PIL import Image
from plotly.subplots import make_subplots
import plotly.graph_objects as go


st.set_page_config(page_title='Covid 19 data Analysis by John Hopkins University')
st.header('Covid 19 Data Analysis by John Hopkins university ')
st.subheader('Confirmed global data analytics ')

st.markdown(""" Representation of global dataset and its basic insights """)

# ---Load Dataframe
excel_file= 'Confirmed_global.xlsx'


df= pd.read_excel(excel_file,
                #usecols= 'B:C',
                header=3
                )

dfcases= pd.read_excel(excel_file,
                usecols= 'B:D',
                header=3
                )


st.dataframe(df)

st.subheader("The below bar graph represents number of cases in each country and region")

bar_chart = px.bar(df,
                    title='number of countries infected',
                    x='Algeria',
                    y=28.0339)

st.plotly_chart(bar_chart)

st.subheader("now lets display basic analysis of data ")

st.subheader("Display whole dataset ")

# Display Data
if st.checkbox("Display Data"):
    st.write(df.head())

st.subheader("Select particular radio button to find length of rows and columns ")
# Rows and Columns
dimensions = st.radio("What Dimension Do You Want to Show?", ("Rows", "Columns"))
if dimensions == "Rows":
    st.text("Showing Length of Rows")
    df.shape[0]

if dimensions == "Columns":
    st.text("Showing Length of Columns")
    excel_file.shape[1]

st.subheader("General statistics by summary of dataset")
# General Statistics 
if st.checkbox("Show Summary of Dataset"):
    st.write(df.describe())

#Sidebar
st.sidebar.title("Dataset")
st.sidebar.text_input("Link to data",("https://github.com/CSSEGISandData/COVID-19/blob/master/csse_covid_19_data/csse_covid_19_time_series/time_series_covid19_confirmed_global.csv"))

st.sidebar.title("Options")
option = st.sidebar.selectbox("which Dashboard?", ('Donut Chart', 'Bar Chart'))

covid_cases = df[['Algeria']].value_counts().to_frame().reset_index().rename(columns={0:'counts'})
covid_cases['Algeria'] = covid_cases['Algeria'].apply(lambda x : str(x) + ' ' + "Cases")
labels = list(covid_cases['Algeria'])

st.subheader("Graphical representation of dataset through charts ")
if option == 'Donut Chart':
    
    fig = make_subplots(rows=1, cols=2, specs=[[{'type':'domain'}, {'type':'domain'}]])
    fig.add_trace(go.Pie(labels=labels, textinfo='none',values=covid_cases['counts'], name="Algeria"),
                row=1, col=1)
    fig.update_traces(hole=.5, hoverinfo="label+value+name")
    fig.update_layout(
        title="Confirmed global cases",
        title_x=0.3,
        legend=dict(
            x=0.5,
            y=1,
            traceorder="reversed",
            title_font_family="Times New Roman",
            font=dict(
                family="Courier",
                size=15,
                color="black")
        ))
    st.plotly_chart(fig)

dataframe=pd.read_excel(excel_file)
if option == 'Bar Chart':
    category_tr = df.groupby(by='Unnamed: 0')[28.0339].sum().to_frame().sort_values(28.0339).reset_index()

    fig1 = px.bar(category_tr, x='Unnamed: 0', y=28.0339,
                color='Unnamed: 0')
    fig1.update_layout(showlegend=False,
                    title="Covid confirmed cases on provinces/state",
                    title_x=0.5,
                    xaxis_title='Provinces/state',
                    yaxis_title='Long lived people')
    st.plotly_chart(fig1)

