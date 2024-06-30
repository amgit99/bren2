```dataviewjs  
function progress(goal, parts, completed = 0) {
	let value = completed/parts * 100
	return `<tr> <td> <span style="font-size: 16px">${goal}</span> <br> <progress style="font-size: 18px" value="${parseInt(value)}" max="100"></progress>       <span style="font-size: 16px">${parseInt(value)}%</span> </td> </tr> `
}

dv.span(`
<table>  
  <tr> 
	  <th> <span style="color:#e1db3d">Goals</span> </th>
  </tr>  
  ${progress("C++ Course", 12, 3)}
  ${progress("DDIS Book", 12, 1)}
  ${progress("The Grind", 250, 141)}
  ${progress("Udemy NEXT.js", 16, 1)}
</table>
`)
```
