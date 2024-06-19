# BeamAnalysis
#Python code that allows calculations and diagrams of BM and SF in a simply supported beam
import matplotlib.pyplot as plt
import numpy as np

def plot_beam(length, point_loads, udl_loads):
    fig, ax = plt.subplots(figsize=(12, 6))

    # Draw beam as a straight line
    ax.plot([0, length], [0, 0], 'k-', lw=2, label='Beam')

    # Draw supports (roller at left end, hinge at right end)
    ax.plot([0], [0], marker='o', markersize=10, color='green', label='Roller Support (Left End)')
    ax.plot([length], [0], marker='s', markersize=10, color='red', label='Hinge Support (Right End)')

    # Label beam length below the beam
    ax.text(length / 2, -0.5, f'Beam Length: {length} m', ha='center')

    # Plot point loads
    for load in point_loads:
        position = load['position']
        magnitude = load['magnitude']
        ax.annotate(f'{magnitude} kN', xy=(position, 0), xytext=(position, 2), ha='center', va='bottom',
                    arrowprops=dict(arrowstyle='->', color='black'))
        ax.plot([position, position], [2, 0], 'k--')

    # Plot uniformly distributed loads (UDLs)
    for udl in udl_loads:
        start_position = udl['start_position']
        end_position = udl['end_position']
        magnitude = udl['magnitude']
        ax.add_patch(plt.Rectangle((start_position, 0), end_position - start_position, -magnitude,
                                   facecolor='orange', edgecolor='black', alpha=0.5))
        ax.annotate(f'{magnitude} kN/m', xy=((start_position + end_position) / 2, 0), xytext=((start_position + end_position) / 2, 2),
                    ha='center', va='bottom', arrowprops=dict(arrowstyle='->', color='black'))
        ax.plot([start_position, start_position], [2, 0], 'k--')
        ax.plot([end_position, end_position], [2, 0], 'k--')

    ax.set_xlim(-1, length + 1)
    ax.set_ylim(min(-10, min([load['magnitude'] for load in point_loads])), max(10, max([load['magnitude'] for load in point_loads])))

    ax.axhline(0, color='black', linewidth=1)
    ax.set_xlabel('Length (m)')
    ax.set_ylabel('Load Intensity (kN or kN/m)')
    ax.set_title('Beam with Applied Loads')
    ax.legend(loc='upper left')
    ax.grid(True)

    return fig, ax

# Example inputs
length = 10.0
point_loads = [{'position': 2.0, 'magnitude': 20.0}, {'position': 7.0, 'magnitude': 15.0}]
udl_loads = [{'start_position': 3.0, 'end_position': 6.0, 'magnitude': 10.0}]

# Plotting the beam with loads
fig, ax = plot_beam(length, point_loads, udl_loads)

plt.show()
