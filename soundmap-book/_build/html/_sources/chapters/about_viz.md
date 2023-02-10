# About Data Visualizations

## Colombia Map Chart

![colombia_map](https://raw.githubusercontent.com/LauraTrujilloT/mapa_sonoro_book/main/soundmap-book/col_map.png)

```python
 col_fig = px.scatter_geo(
                        col_df.dropna(), 
                        lat=col_df["municipio_latitud"].astype(float), 
                        lon=col_df["municipio_longitud"].astype(float),     
                        color=col_df["vitalidad"], 
                        size=bubble_size,
                        size_max=bubble_max,
                        height=1000,
                        center = {"lat": 4.0, "lon": -72.5},
                        opacity=1.,
                        scope='south america',
                        color_discrete_map={
                                            'Critically Endangered':'#A90639',#'rgb(250, 87, 98)',
                                            'Endangered': '#e60049',#'#96dae8'
                                            'Vulnerable': '#FFE15D',#'#FFF600',#'#FFE15D',
                                    },
                        custom_data=[col_df["n_hablantes"], col_df['n_habitantes'], 
                            col_df['familia_linguistica'], col_df['nombre_lengua'],
                            col_df['pct_hablantes'], col_df['pct_habitantes'], bubble_size, col_df['lengua_ratio'],
                            col_df['properties.NOMBRE_DPT']
                            ],
                    )
    
    choropleth_col = px.choropleth(
                                col_geojson_,
                                geojson=deptos,
                                locations='properties.DPTO',
                                featureidkey='properties.DPTO',
                                projection='mercator',
                                height=1000,
                                color_discrete_sequence=[colormap]
                            )
    choropleth_col.update_traces(
        hoverinfo='skip',
        hovertemplate=None,
        marker_line_width=0.5,
        marker_line_color='#8daad6')
    choropleth_col.update_geos(fitbounds='locations',visible=False)
    col_fig.update_geos(fitbounds='locations', visible=False, bgcolor='#f8f9fa')

    col_fig.update_traces(
                    hovertemplate="<br>".join([
                        "<I> %{customdata[8]}</I>",
                        "<b> %{customdata[2]} Family</b> - %{customdata[3]}",
                        "<b>Speakers:</b> %{customdata[0]:,} (%Total %{customdata[5]:.2}%)", 
                        "<b>Locals:</b> %{customdata[1]:,} (%Total %{customdata[4]:.2}%)",
                        f"<b>{legend_title}</b>: " + "%{customdata[6]:.2}"
                        ]),
                    marker_sizemin=4
    )
    col_fig.add_trace(choropleth_col.data[0])
    for i, frame in enumerate(col_fig.frames):
        col_fig.frames[i].data += (choropleth_col.frames[i].data[0],)

    col_fig.update_layout(
        margin={"r": 0, "t": 0, "l": 0, "b": 0}, 
        title={
            'text': 'Colombia Map',
            'x':0.2,
            'y':0.98,
            'xanchor':'center'
            },
        showlegend=legend_switch,
        legend={
            'orientation':'h',
            'font': {'size':14},
            'title':None,
            'xanchor':'left'
        },
        hoverlabel={
            'font':{'size':15},
            'bgcolor':'rgba(255,255,255,0.75)'
            },
        paper_bgcolor='#f8f9fa'
        )
```

## Indicators Chart

![indicator](https://raw.githubusercontent.com/LauraTrujilloT/mapa_sonoro_book/main/soundmap-book/indicators.png)

**Code Snippet**

```python
    col_df = col_dataframe()

    trace1 = go.Indicator(
        mode="number",    
        value=col_df.n_hablantes.sum(),    
        domain={'x': [0., 0.33], 'y': [0.0, 1.]},  
        number={'valueformat':',r', 'font':{'size':35}},  
        title={'text': "Total Speakers", 'font':{'size':15}},
        )
    trace2 = go.Indicator(
        mode="number",    
        value=col_df.nombre_lengua.nunique(),    
        domain={'x': [0.33, 0.66], 'y': [0., 1.]},  
        number={'font':{'size':35}},  
        title={'text': "Languages", 'font':{'size':15}})
    trace3 = go.Indicator(
        mode="number",    
        value=col_df.n_habitantes.sum(),    
        domain={'x': [0.66, 1.0], 'y': [0., 1.00]},  
        number={'valueformat':',r', 'font':{'size':35}}, 
        title={'text': "Total Locals", 'font':{'size':15}})

    # layout and figure production
    layout = go.Layout(
                    plot_bgcolor='rgba(0,0,0,0)',
                    xaxis={'showgrid': False, 'showticklabels':False, 'range':[-1,1]},
                    yaxis={'showgrid': False, 'showticklabels':False, 'range':[0,1]},
                    margin=dict(l=20, r=20, t=20, b=20),
                    autosize=True,
                    #height=100
                    )
    fig = go.Figure(data = [trace1, trace2, trace3], layout = layout)
    fig.add_annotation(
                text='out of <b>48,258,494</b> colombians <I>(DANE, 2018 Census)</I>',
                x=0.2,
                y=0.1,
                xref='x',
                yref='y',
                showarrow=False,
    )
```