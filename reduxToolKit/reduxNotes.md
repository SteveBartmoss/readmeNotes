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