```js
const express = require('express');

const app = express();
const PORT = 3000;

/* functions */
function isEven(arr){
    return arr.length % 2 === 0;
}

function mean(arr){
    let sum = 0;
    const length = arr.length;
    for(let i = 0; i < length; i++){
        sum += arr[i];
    }
    return sum/length;
}


function median(arr){
    let l = arr.length;
    if(isEven(arr)){
        let index_1 = arr[Math.floor(l/2)];
        let index_2 = arr[Math.floor(l/2-1)];
        return (index_1+index_2)/2;
    }
    else{ // presumably odd number of values in arr
        return arr[Math.floor(l/2)];
    }
}

function mode(arr){
    const dict = {};
    for(let i=0; i<arr.length; i++){
        if(dict[arr[i]]){
            dict[arr[i]] += 1;
        }
        else{
            dict[arr[i]] = 1;
        }
    }
    // console.log(dict);
    keys = Object.keys(dict);
    let high = 0;
    for(let k of keys){
        if(dict[k] > high){
            high = k;
        }
    }
    return high;
}

app.use(express.json());

app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).send('Something went wrong!');
})

/* Routes */
app.get('/', (req, res) => {
    res.send('Goto /mean, /median, /mode');
})

// routes
app.get('/api/mean', (req, res) => {
    try{
        const numsString = req.query.nums; 
        const numsArray = numsString.split(',').map(Number); 
        const numsArrayMean = mean(numsArray);
        res.json({ operation:'mean',
        value: numsArrayMean });
    }
    catch(e){
        res.send('ERROR: No values in array');
    }
});

app.get('/api/median', (req, res) => {
    try{
        const numsString = req.query.nums;
        const numsArray = numsString.split(',').map(Number);
        const numsArrayMedian = median(numsArray);
        res.json({operation: 'median',
        value: numsArrayMedian});
    }
    catch(e){
        res.send('ERROR: No values in array');
    }
});
app.get('/api/mode', (req, res) => {
    try{
        const numsString = req.query.nums;
        const numsArray = numsString.split(',').map(Number);
        const numsArrayMode = mode(numsArray);
        res.json({operation: 'mode',
        value: numsArrayMode});
    }
    catch(e){
        res.send('ERROR: No values in array');
    }
});

app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});
```
