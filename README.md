
# VEX Collection

This is my journey to learning vex and wanted to share my vex snippets along the way.


### @Shop_Materialpath to Groups (getting shaders into Solaris)
``` c#
//strings to groups
string name = split(s@shop_materialpath, '/')[-1];
name = re_replace("[^0-9a-zA-Z\\.]", "_", name);
string groupName = name + "_mat";
setprimgroup(0, groupName, @primnum, 1, "set");
```

### Camera Clipper
``` c#
//Camera Clipper
string cam = chs("cam_path");

@P = fromNDC(cam, @P);
```

### Carve Curve from Mask
``` c#
//Carve Curve from Mask
float map = @curveu;
float p = chf("position");
float t = chf("transition_width");


p = fit01(p, -t, 1);
float mask = fit(map, p, p+t, 1, 0);

float ramp = chramp("ramp", mask);
f@mask = ramp;
if(@mask < 0.1) removepoint(0, @ptnum);
```

### Direction to Point
``` c#
//Direction to Point
vector a = @P;
vector b = point(1,'P',0);

@N = b-a;
```

### Hue Shift
``` c#
//Hue Shift
float hueShift = ch("hue_shift");
@Cd = hsvtorgb(set(hueShift, 1, 1));
```

### Make Transform
``` c#
//Make Transfrom

vector t=chv("translate"), r =chv("rotate"), s=chv("scale"),p=chv("pivot"), pr=chv("pivotRotation");

matrix transform = maketransform(0,0,t,r,s,p,pr);

matrix3 ident = maketransform({0,0,1},{0,1,0});

float amount = ch("amount");

rotate(ident,radians(amount),{0,1,0});

@P *= transform + ident;
//@P *= ident;
```

### Mapped Transition
``` c#
//Mapped Transition
float map = f@map;
float p = chf("position");
float t = chf("transition_width");
t = max(t,  0.0001);

p = fit01(p, -t, 1);
float mask = fit(map, p, p+t, 1, 0);
mask = chramp("ramp", mask);
f@mask = mask;
```

### Primuv Attach to Point
``` c#
//Primuv attach point
vector pos = primuv(1, "P", chi('prim_number'), chv("uv_pos"));
@P = pos;
```

###  Pulse Effect
``` c#
//Pulse Effect
float pulseFrequency = ch("pulse_frequency");
float pulseAmplitude = ch("pulse_amplitude");
float min = ch('min');
float max = ch('max');
@P *= fit01((pulseAmplitude * cos(pulseFrequency*@Time)), min, max);
```

### Random Particle Life with Min and Max
``` c#
//Particle life Min Max
f@life = fit01(rand(@ptnum),chf('min'),chf('max'));
```



