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

### readin csv file

from astropy.table import Table, vstack
import os

sour_tab = Table.read('../pos_gw_all.csv')

for obj in sour_tab[:]:
    obj_name=obj['name']
    index=obj['index']

### write and read *csv ###########################################

import numpy as np
from astropy.io import ascii
from astropy.table import Table
data = Table()
data['RXGCC'                     ] = ['{:6s}'.format(x)   for x in idd_rxgcc]
data['RAJ2000[deg]'              ] = ['{:7.3f}'.format(x) for x in ra_rxgcc]
data['DEJ2000[deg]'              ] = ['{:7.3f}'.format(x) for x in dec_rxgcc]
data['Ext[arcmin]'               ] = ['{:7.2f}'.format(x) for x in extent_rxgcc]
data['Ext,ml'                    ] = ['{:7.2f}'.format(x) for x in extml_rxgcc]
#data['Alias'                     ] = alias_rxgcc
#data['Tile'                      ] = tile_rxgcc
ascii.write(data, 'table_rxgcc.csv', delimiter='&', overwrite=True)

import numpy as np
from astropy.io import ascii
from astropy.table import Table
t = Table.read('table_rxgcc.csv', format='ascii', delimiter='&')
t['RAJ2000[deg]'] # float array

### readin *txt, *dat, *cat file 
# the first line of the *.cat is `# ra dec ...`
f = open('sdss_legacy_hscoverlap_mcut11.0.cat', 'r')
ra, dec, z, mstar, mstar_err = np.loadtxt(f, usecols=(0, 1, 2, 3, 4), unpack=True, comments='#', delimiter=None)
f.close()

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

### read and write npz, npy, h5 file
#################### readin *.npz file ############################################

import numpy as np 
f=np.load("nz_compare_knnsom.npz") # NPZ file is a NumPy Zipped Data.
print(f.files)         # to show columns
z_tr=f['z_tr'])     # to get columns 
np.size(f['z_tr'])     # 150
type(f['z_tr'])     # <class 'numpy.ndarray'>

##################################################################################

import numpy as np
x=np.ndarray(shape=(3,10), dtype=float, order='F') # type(ndarray)
with open('test.npy', 'wb') as f:
    np.save(f, x)

with open('test.npy', 'rb') as f:
    a = np.load(f)

#################### write h5py file ############################################
import h5py  # 导入工具包
import numpy as np

imgData = np.zeros((30, 3))
with h5py.File('HDF5_FILE.h5', 'w') as f:
    f.create_dataset('data', data=imgData)
    f.create_dataset('labels', data=range(100))

print '*.h5 Created.'

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


### Convert UTC time to mjd, convert mjd to UTC #############################

from astropy.time import Time

times="2020-01-01"
t = Time(times, format='isot', scale='utc')
date=t.mjd # 58849.0

mjd='58776'
t=Time(mjd,format='mjd')
t.format = 'isot'
t.value # '2019-10-20T00:00:00.000'



### flat: make 2-d array to 1-d array  ##############################
from numpy import matrix as mat

a = [[1,3],[2,4],[3,5]]
a = mat(a)
y = a.flatten()     # matrix([[1, 3, 2, 4, 3, 5]])
y = a.flatten().A     # array([[1, 3, 2, 4, 3, 5]])

### convert degree to radian as unit #################
np.deg2rad(dec[i])
