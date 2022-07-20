# Atomisation-Energy-of-CO2
from ase.build import molecule
from gpaw import GPAW

a = 8.0
h = 0.2

energies = {}
with open(f'results-{h:.2f}.txt', 'w') as resultfile:

    for name in ['CO2', 'C', 'O']:
        system = molecule(name)
        system.set_cell((a, a, a))
        system.center()   # used to centre the system in the above cell
    
        calc = GPAW(h=h,
                    txt=f'gpaw-{name}-{h:.2f}.txt')
        if name == 'H' or name == 'O':
            calc.set(hund=True)    # used to apply Hund's rule 
    
        system.calc = calc
    
        energy = system.get_potential_energy()
        energies[name] = energy
        print(name, energy, file=resultfile)
    
    e_atomization = energies['CO2'] - 1 * energies['C'] - 2 * energies['O']
    print(e_atomization, file=resultfile)
    ########################################################################
    This calculates the atomization Energy as
    CO2 -25.553967350409792
    C -1.1770525360113773
    O -1.830234131376214
      -20.716446551645987
