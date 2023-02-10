# About Data Visualizations

## Colombia Map Chart

[colombia_map](col_map.png)

## Indicators Chart

[indicator](indicators.png)

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