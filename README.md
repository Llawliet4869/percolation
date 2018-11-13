# percolation
'''
This program will simulate a percolation structure first using random number generators 
and then by Leath's algorithm which grows clusters of interconnected materials.
'''

import numpy as np
import matplotlib.pyplot as plt

#We initialize our square lattice
L = 11
js = np.arange(L)
ls = np.arange(L)
p = 0.59275
np.random.random()
lattice = np.zeros((L,L))
plt.imshow(lattice)

#At each site, we compare a random number to the concentration. 
#if the number is less than the concentration, we set it to one, if not, then 0
for l in ls:
    for j in js:
        r = np.random.random()
        if r < p:
            lattice[l,j] = 1
        else:
            lattice[l,j] = 0
            
plt.imshow(lattice, interpolation = "nearest")

# We initialize our square lattice by labelling all sites of the lattice boundary as vacant 
# (set to a value of 2) the central site to be occupied (set to a value of 1) and all other sites 
# are undefined (set to a value of 0)

L = 11
p = 0.59275
lattice = np.zeros((L,L))
lattice[0,:] = 2*np.ones(len(lattice[0,:]))
lattice[L-1,:] = 2*np.ones(len(lattice[L-1,:]))
lattice[:,0] = 2*np.ones(len(lattice[:,0]))
lattice[:, L-1] = 2*np.ones(len(lattice[:, L-1]))
lattice[5,5] = 1

ls = np.arange(L)
js = np.arange(L)

plt.imshow(lattice)

# We define a function that determines the nearest undefined neighbors of all occupied cells
def neighbors():
    nearest = []
    for l in ls:
        for j in js:
            if lattice[l,j] == 1:
                if lattice[l+1, j] == 0:
                    nearest.append((l+1,j))
                if lattice[l-1, j] == 0:
                    nearest.append((l-1,j))
                if lattice[l, j+1] == 0:
                    nearest.append((l,j+1))
                if lattice[l, j-1] == 0:
                    nearest.append((l,j-1))
    return nearest

#we iterate until the number of undefined neighbors of all occupied cells is zero
while len(neighbors()) > 0:
    neighbors()
    #we pick an undefined site adjacent to the cluster
    index = neighbors()[np.random.choice(np.array(len(neighbors())))]
    #we draw a random number and compare it with our concentration
    #if the number is less than the percolation, we set the undefined site to occupied, otherwise, it is vacant 
    r = np.random.random()
    if r < p:
        lattice[index[0], index[1]] = 1
    else:
        lattice[index[0], index[1]] = 2   

plt.imshow(lattice)

