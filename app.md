<img width="1917" height="872" alt="tab1" src="https://github.com/user-attachments/assets/e1560210-dbcd-4041-b582-67e9b3ae1a0d" />

``` python
## imports
import streamlit as st
import pandas as pd
import plotly.express as px

## plot functions
def layout(fig):
	fig.update_layout(height=275, margin=dict(l=24, r=24, t=24, b=24))
	fig.update_traces(cliponaxis=False, texttemplate='%{text:$,.0f}')
	return fig


def fig_plotter(df):
	# bar chart
	df_group = df.groupby("Region")["Sales"].sum().reset_index()
	bar = px.bar(data_frame=df_group,
			 x="Region",
			 y="Sales",
			 text="Sales",
			 color="Sales",
			 hover_data={"Sales":":$,"},
			 template="seaborn",
			 orientation="v",
			 title="Sales Sum by Region").update_traces(textposition="outside")

	# line chart
	df_group2 = df.groupby(["Month"])["Sales"].sum().reset_index()
	line = px.line(data_frame=df_group2,
			   x="Month",
			   y="Sales",
			   text="Sales",
			   hover_data={"Sales":":$,"},
			   template="seaborn",
			   markers=True,
			   title="Sales Sum by Month").update_traces(textposition="top center")

	# bar2 chart
	df_group3 = df.groupby("Region")["Expenses"].mean().reset_index()
	bar2 = px.bar(data_frame=df_group3,
			  x="Region",
			  y="Expenses",
			  text="Expenses",
			  color="Expenses",
			  hover_data={"Expenses":":$,.2f"},
			  template="seaborn",
			  orientation="v",
			  title="Expenses Average by Region").update_traces(textposition="outside")

	# line2 chart
	df_group4 = df.groupby(["Month"])["Expenses"].sum().reset_index()
	line2 = px.line(data_frame=df_group4,
			    x="Month",
			    y="Expenses",
			    text="Expenses",
			    hover_data={"Expenses":":$,"},
			    template="seaborn",
			    markers=True,
			    title="Expenses Sum by Month").update_traces(textposition="top center")

	return layout(bar), layout(line), layout(bar2), layout(line2)


## read data into csv
df = pd.read_csv("sales_data.csv")

## page configuraiton
st.set_page_config(page_title="Dashboard", layout="wide", initial_sidebar_state="collapsed")

## sidebar filters
with st.sidebar:
	st.title("Filter Options")
	regions_selected = st.multiselect(label="Regions",
									  help="Please choose one or more regions to filter.",
									  width="stretch", disabled=False,
									  options=df.Region.unique(),
									  default=df.Region.unique(),
									  filter_mode="contains")

	month_selected = st.selectbox(label="Months",
								  help="Please choose one month to filter.",
								  options=["All"] + df.Month.unique().tolist(),
								  index=0,
								  filter_mode="contains")

# filtered dataframe manipulation
if month_selected!= "All":
	df_filtered = df.query("Region in @regions_selected and Month==@month_selected")
else:
	df_filtered = df.query("Region in @regions_selected")

df_difference = df_filtered.assign(Difference=df_filtered["Sales"] - df_filtered["Expenses"])

## plots
bar, line, bar2, line2 = fig_plotter(df_filtered)

## layout
tab1, tab2 = st.tabs(tabs=["Sales Overview", "Expenses Analysis"], default="Sales Overview")
with tab1:
	t1container1 = st.container(horizontal=True, height=300)
	with t1container1:
		with st.expander(label="See filtered dataframe:", type="compact", expanded=False):
			data_selected = st.dataframe(df_filtered, hide_index=True, height=240)
		bar_selected = st.plotly_chart(bar, theme="streamlit")

	t1container2 = st.container(horizontal=True, height=300)
	with t1container2:
		with st.expander(label="See difference dataframe:", type="compact", expanded=False):
			st.dataframe(df_difference, hide_index=True, height=240)
		line_selected = st.plotly_chart(line, theme="streamlit")

with tab2:
	t2container1 = st.container(horizontal=True, height=300)
	with t2container1:
		with st.expander(label="See filtered dataframe:", type="compact", expanded=False):
			data_selected = st.dataframe(df_filtered, hide_index=True, height=240)
		bar2_selected = st.plotly_chart(bar2, theme="streamlit")

	t2container2 = st.container(horizontal=True, height=300)
	with t2container2:
		with st.expander(label="See difference dataframe:", type="compact", expanded=False):
			st.dataframe(df_difference, hide_index=True, height=240)
		line2_selected = st.plotly_chart(line2, theme="streamlit")
```
<img width="1917" height="870" alt="tab2" src="https://github.com/user-attachments/assets/a47782ac-36c7-457c-a8a9-62769705ff44" />
