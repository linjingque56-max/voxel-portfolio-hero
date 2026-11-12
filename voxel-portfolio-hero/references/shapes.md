# Voxel shape recipes

All snippets go inside the `<script>` of the template, after `rnd` is defined and before the `S=[...]` line. They push `[x,y,z,key]` into an array; list the arrays in `S` and their names in `NM` (3 to 5 shapes work well; update the HUD module text to match, and the `%3` in `si=cyc%3,ni=(si+1)%3` to the shape count).

Add colour keys to `PAL` as `key:[r,g,b]`. Template keys: `r` brick red, `s` salmon, `d` dark brown, `t` tan, `w` off-white, `g` grey (also used for dust, keep it).

## Included in the template
- `tower(o,x0,y0,w,d,h)`: hollow building, dark walls, alternating red/tan windows on the visible sides, grey roof.
- Monogram: 3×5 bitmap letters scaled ×2, extruded y=7..8 (`L` object).
- Cloud: noisy ellipsoid of white voxels at z 9..12 over a small platform, plus falling red cubes.

## White ghost (x–z plane, y=8..9)
```js
const Gh=[];for(let x=2;x<14;x++)for(let z=0;z<13;z++){const dx=x-7.5;
 let on=z>=6?dx*dx+(z-6.5)**2<=37:true;if(z<2&&![2,5,6,9,10,13].includes(x))on=false;if(!on)continue;
 for(let y=8;y<10;y++){let c=rnd()<.05?'g':'w';if(y==9&&z>=7&&z<=8&&(x==5||x==6||x==9||x==10))c='d';
  if(y==9&&z==5&&(x==4||x==11))c='s';if(y==9&&z==4&&(x==7||x==8))c='s';Gh.push([x,y,z,c])}}
```

## Rainy forest (needs keys v:[104,138,92], u:[62,96,70], b:[120,150,170])
```js
const Fo=[];
[[3,3],[7,2],[11,3],[2,8],[6,7],[10,8],[4,12],[12,12]].forEach(([x,y],i)=>{const h=2+i%2;
 for(let z=0;z<h;z++)Fo.push([x,y,z,'d']);
 for(let dx=-2;dx<=2;dx++)for(let dy=-2;dy<=2;dy++){if(Math.abs(dx)==2&&Math.abs(dy)==2)continue;
  Fo.push([x+dx,y+dy,h,(dx+dy)%3?'v':'u']);if(Math.abs(dx)<2&&Math.abs(dy)<2)Fo.push([x+dx,y+dy,h+1,'v'])}
 Fo.push([x,y,h+2,'u'])});
for(let x=3;x<13;x++)for(let y=4;y<12;y++)for(let z=11;z<14;z++){const v=((x-7.5)/5)**2+((y-7.5)/4)**2+((z-12.5)/1.7)**2+(rnd()-.5)*.35;if(v<1)Fo.push([x,y,z,rnd()<.1?'g':'w'])}
for(let i=0;i<22;i++){const x=3+(rnd()*10|0),y=4+(rnd()*8|0),z=7+(rnd()*3|0);Fo.push([x,y,z,'b'],[x,y,z+1,'b'])}
```

## Floating castle (same extra keys)
```js
const Ca=[];
[[5,6],[4,5],[3,3.6],[2,2.2],[1,1]].forEach(([z,r])=>{for(let x=0;x<G;x++)for(let y=0;y<G;y++)if(Math.hypot(x+.5-8,y+.5-8)<=r)Ca.push([x,y,z,z==5?(rnd()<.12?'u':'v'):(rnd()<.2?'d':'t')])});
for(let z=6;z<11;z++)for(let x=7;x<11;x++)for(let y=7;y<11;y++){if(x>7&&x<10&&y>7&&y<10)continue;Ca.push([x,y,z,z%2==0&&(x+y)%2&&(x==10||y==10)?'d':'w'])}
for(let x=7;x<11;x++)for(let y=7;y<11;y++)Ca.push([x,y,11,'r']);
for(let x=8;x<10;x++)for(let y=8;y<10;y++)Ca.push([x,y,12,'r']);
Ca.push([8,8,13,'r'],[8,8,14,'s']);
[[4,4],[10,11]].forEach(([x,y])=>{for(let z=6;z<10;z++)for(let a=0;a<2;a++)for(let b=0;b<2;b++)Ca.push([x+a,y+b,z,z==8&&a&&b?'d':'w']);
 for(let a=0;a<2;a++)for(let b=0;b<2;b++)Ca.push([x+a,y+b,10,'r']);Ca.push([x,y,11,'r'])});
```

## Logo from a screenshot: three methods
1. **Tiles / rectangles** (multi-colour mark). Measure each tile in the image, convert to a 12-column grid, paint rectangles in z-order into a `{ "row,col": key }` map, then extrude. Tiles that overlap others get one extra y layer so they stand proud:
```js
const F_=(g,c0,c1,r0,r1,k)=>{for(let r=r0;r<=r1;r++)for(let c=c0;c<=c1;c++)g[r+','+c]=k};
const Q={};F_(Q,3,8,0,3,'Y');F_(Q,7,11,3,6,'B');F_(Q,0,3,3,6,'R'); /* …more rectangles… */
const QM=[];for(const p in Q){const[r,c]=p.split(',').map(Number),k=Q[p];
 [7,8].concat('KP'.includes(k)?[9]:[]).forEach(y=>QM.push([2+c,y,14-r,k]))}
```
2. **ASCII bitmap** (icon with symmetry). One string per row, `.` empty, a letter per colour; width ≤ 15, rows ≤ 16:
```js
TR.forEach((s,r)=>[...s].forEach((k,c)=>{if(k!='.')for(let y=7;y<9;y++)Tr.push([c+1,y,15-r,k])}));
```
3. **Italic wordmark**. 7-row bitmaps per letter, widths 5/5/4 placed at x=0/5/10, sheared with `Math.round((6-r)/3)` so the top shifts right by 2; extrude y=6..9 for a chunky look; total width stays ≤ 16.

Measure proportions from the image, never guess colours; sample or eyeball them and add them to `PAL`.
