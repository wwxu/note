### read file 

import re
import string
import numpy as np

f=open("ext_extent_unp.dat", "r"); lines=f.readlines(); f.close()
ynames=lines[0].split()
y={}
for name in ynames:
        y[name]=[]

for line in lines[1:]:
        yvalues=[x for x in line.split()]
        if len(yvalues)>0: 
                i=0
                for name in ynames:
                        if i==0:
                                y[name].append(yvalues[i]); i+=1;
                        else:
                                y[name].append(float(yvalues[i])); i+=1

idd=y['fcs'];                   idd2=y['idd'];          ra=y['ra(deg)'];                dec=y['dec(deg)'];      #0/1/2/3

n_det=np.size(idd)
print("number of detections:", n_det)



### readin fits file

import re
import astropy.io.fits as fits
import numpy as np

dat=fits.open("gal_decals_dr3_calib.fits")
a     = dat[1].data 
ra    = a['ra']  
dec   = a['dec']
zph   = a['photo_z']

### write a fits file 

c1 = fits.Column(name='Name',    array=idd,    format='A20', ascii=True)
c2 = fits.Column(name='RAJ2000', array=ra,     format='D',   ascii=True)
c3 = fits.Column(name='DEJ2000', array=dec,    format='D',   ascii=True)
c4 = fits.Column(name='z',       array=z,      format='D',   ascii=True)
c5 = fits.Column(name='deltaz',  array=deltaz, format='D',   ascii=True)
t = fits.BinTableHDU.from_columns([c1, c2, c3, c4, c5])
t.writeto('Hasselfield2013_68.fits', overwrite=True)

### remove all repetitive one and reserve only one in dis<dis  ###########################
def self_match(ra1,dec1,z1,dis):
    # to remove all repetitive one and reserve only one in dis<dis
    c = SkyCoord(ra=ra1*u.degree, dec=dec1*u.degree)
    catalog = SkyCoord(ra=ra1*u.degree, dec=dec1*u.degree)
    idxc, idxcatalog, d2d, d3d = catalog.search_around_sky(c, dis*u.deg)
    n_match=np.size(d2d); size1=np.size(ra1)
    idxc=list(idxc)
    index_rep=[];
    for j in range(n_match-1):
        if idxcatalog[j]<idxc[j]:
            index_rep.append(idxcatalog[j])
    index_rep=set(index_rep)
    index_rep=list(index_rep)
    index_rep=np.sort(index_rep)
    size_rep=np.size(index_rep);
    i=0;
    ra_new=list(ra1);
    dec_new=list(dec1);
    z_new=list(z1);
    for k in range(size_rep):
        ra_new.pop(index_rep[k]-i)
        dec_new.pop(index_rep[k]-i)
        z_new.pop(index_rep[k]-i)
        i+=1
    if size_rep+np.size(z_new)==size1:
        print("check done!")
    ra1=ra_new
    dec1=dec_new
    z1=z_new
    c = SkyCoord(ra=ra1*u.degree, dec=dec1*u.degree)
    catalog = SkyCoord(ra=ra1*u.degree, dec=dec1*u.degree)
    idxc, idxcatalog, d2d, d3d = catalog.search_around_sky(c, dis*u.deg)
    n_match=np.size(d2d); size_new=np.size(ra1)
    if n_match==size_new:
        print("double check done!")
    x=1-size_new*1.0/size1
    x=round(x,3)
    print(size1,"->",size_new, " (rep:"+str(x)+")")
    return ra_new, dec_new,z_new

ra_sp_new, dec_sp_new, z_sp_new= self_match(ra_sp, dec_sp, z_sp,dis_lim)
