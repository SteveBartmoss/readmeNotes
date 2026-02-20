# Instalacion

Para instalar reduxtoolkit se usan los siguiente comandos

```bash
npm install @reduxjs/toolkit
```

Sin fueran necesarios los para react se usa el siguiente comando 

```bash
npm install react-redux
```

Con esto tenemos lo necesario para trabajar con reduxtoolkit en una aplicacion de react

# Creacion de un slice

La estructura basica de un slice de redux es la siguiente

```js
import { createSlice } from "@reduxjs/toolkit"

const initialState = {
    items: [],
}

const itemsSlice = createSlice({
    name: 'licences',
    initialState,
    reducers: {
        setItems(state, action) {
            state.items = action.payload
        }
    }
})

export const {
    setItems,
} = itemsSlice.actions

export default itemsSlice.reducer
```

Con lo anterior se tiene una estructura basica de un slice en redux, pero para poder utilizar lo o usar cualquier otro slice que configuremos, debemos configurar el index para redux tool kit

```js
import { configureStore } from "@reduxjs/toolkit";
import itemsReducer from './itemsReducer'

export const store = configureStore({
    refucer: {
        items: itemsReducer
    },
})
```

Ya que tenemos configurado el index.store, debemos proporcionarlo o inyectarlo a la aplicacion de react de la siguiente manera

```js
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'
import { Provider } from 'react-redux'
import { store } from './store/index.js'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <Provider store={store}>
        <App /> 
    </Provider>
  </StrictMode>,
)
```


Con esto ya podremo usar el slice desde cualquier parte de nuestra aplicacion, pues estamos encerrando toda la aplicacion dentro del provider de redux. 

La forma en la que trabaja Redux toolkit es similar al hook context de react, pero este tiene algunas mejoras con respecto al context, las cuales serían las siguientes


### Inmutabilidad

La idea que tiene redux toolkit, es que el estado nunca se actualiza, siempre se crea uno nuevo, por esta razon es que se suelen hacer este tipo de cosas 

```js
return {
  ...state,
  count: state.count + 1
}
```

En lugar de hacer algo mas simple como esto 

```js
state.count = state.count + 1
```

Eso tiene algunos beneficios como estos 

- Redux compara referencias de objetos lo que permite detectar cambios facilmente

```js
prevState === nextState // false → hubo cambio
```

De otra forma el cambio nunca se escucharia y por lo mismo no podrian ejecutarse procesos siempre que ocurre un cambio

### Mutabilidad


Algo con lo que debemos tener cuidado es con la mutabilidad del estado de reduxtoolkit, por que como tal redux no permite este tipo de cosas, por eso debemos tener cuidado al momento de usar a funcion para establecer el estado
