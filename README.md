# Import necessary Qiskit libraries
from qiskit import QuantumCircuit, Aer, transpile, assemble, execute
from qiskit.visualization import plot_bloch_multivector, plot_histogram
from qiskit.providers.aer import AerSimulator
import matplotlib.pyplot as plt

# Create a quantum circuit with 2 qubits
qc = QuantumCircuit(2)

# Apply a Hadamard gate to qubit 0 (put it in superposition)
qc.h(0)

# Apply a CNOT gate to entangle qubit 0 and qubit 1
qc.cx(0, 1)

# Display the circuit
qc.draw(output='mpl')
plt.show()

# Use the AerSimulator to simulate the circuit
simulator = AerSimulator()

# Transpile the circuit for simulation
qc_sim = transpile(qc, simulator)

# Run the simulation and get the statevector
result = simulator.run(qc_sim).result()
statevector = result.get_statevector()

# Print the statevector (entangled state)
print("Statevector: ", statevector)

# Plot the Bloch sphere to visualize the entanglement
plot_bloch_multivector(statevector)
plt.show()

# Add measurement to the quantum circuit
qc.measure_all()

# Simulate the measurement outcomes
result = execute(qc, backend=simulator).result()
counts = result.get_counts()

# Plot the measurement results (histogram)
plot_histogram(counts)
plt.show()
