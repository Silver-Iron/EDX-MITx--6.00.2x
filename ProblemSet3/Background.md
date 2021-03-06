### Background: Viruses, Drug Treatments, and Computational Models

Viruses such as HIV and H1N1 represent a significant challenge to modern medicine. One of the reasons that they are so difficult to treat is their ability to evolve.

As you may know from introductory biology classes, the traits of an organism are determined by its genetic code. When organisms reproduce, their offspring will inherit genetic information from their parent. This genetic information will be modified, either because of mixing of the two parents' genetic information, or through mutations in the genome replication process, thus introducing diversity into a population.

Viruses are no exception. Two characteristics of viruses make them particularly difficult to treat. The first is that their replication mechanism often lacks the error checking mechanisms that are present in more complex organisms. This speeds up the rate of mutation. Secondly, viruses replicate extremely quickly (orders of magnitude faster than humans) -- thus, while we may be used to thinking of evolution as a process which occurs over long time scales, populations of viruses can undergo substantial evolutionary changes within a single patient over the course of treatment.

These two characteristics allow a virus population to acquire genetic resistance to therapy quickly. In this problem set, we will make use of simulations to explore the effect of introducing drugs on the virus population and determine how best to address these treatment challenges within a simplified model.

Computational modeling has played an important role in the study of viruses such as HIV (for example, see this paper, by MIT graduate David Ho). In this problem, we will implement a highly simplified stochastic model of virus population dynamics. Many details have been swept under the rug (host cells are not explicitly modeled and the size of the population is several orders of magnitude less than the size of actual virus populations). Nevertheless, our model exhibits biologically relevant characteristics and will give you a chance to analyze and interpret interesting simulation data.

### Spread of a Virus in a Person

In reality, diseases are caused by viruses and have to be treated with medicine, so in the remainder of this problem set, we'll be looking at a detailed simulation of the spread of a virus within a person. 

Problem 2: Implementing a Simple Simulation (No Drug Treatment)
15.0 points possible (graded)
We start with a trivial model of the virus population - the patient does not take any drugs and the viruses do not acquire resistance to drugs. We simply model the virus population inside a patient as if it were left untreated.

SimpleVirus class

To implement this model, you will need to fill in the SimpleVirus class, which maintains the state of a single virus particle. You will implement the methods __init__, getMaxBirthProb, getClearProb,doesClear, and reproduce according to the specifications. Use random.random() for generating random numbers to ensure that your results are consistent with ours.

Hint: random seed

The reproduce method in SimpleVirus should produce an offspring by returning a new instance of SimpleVirus with probability: self.maxBirthProb * (1 - popDensity). This method raises a NoChildException if the virus particle does not reproduce. For a reminder on raising execptions, review the Python docs.

self.maxBirthProb is the birth rate under optimal conditions (the virus population is negligible relative to the available host cells so there is ample nourishment available). popDensity is defined as the ratio of the current virus population to the maximum virus population for a patient and should be calculated in the update method of the Patient class.

Patient class

You will also need to implement the Patient class, which maintains the state of a virus population associated with a patient.

The update method in the Patient class is the inner loop of the simulation. It modifies the state of the virus population for a single time step and returns the total virus population at the end of the time step. At every time step of the simulation, each virus particle has a fixed probability of being cleared (eliminated from the patient's body). If the virus particle is not cleared, it is considered for reproduction. The virus population should never exceed maxPop; if you utilize the population density correctly, you shouldn't need to provide an explicit check for this.

Unlike the clearance probability, which is constant, the probability of a virus particle reproducing is a function of the virus population. With a larger virus population, there are fewer resources in the patient's body to facilitate reproduction, and the probability of reproduction will be lower. One way to think of this limitation is to consider that virus particles need to make use of a patient's cells to reproduce; they cannot reproduce on their own. As the virus population increases, there will be fewer available host cells for viruses to utilize for reproduction.

To summarize, update should first decide which virus particles are cleared and which survive by making use of the doesClear method of each SimpleVirus instance, then update the collection of SimpleVirus instances accordingly. With the surviving SimpleVirus instances, update should then call the reproduce method for each virus particle. Based on the population density of the surviving SimpleVirus instances, reproduce should either return a new instance of SimpleVirus representing the offspring of the virus particle, or raise a NoChildException indicating that the virus particle does not reproduce during the current time step. The update method should update the attributes of the patient appropriately under either of these conditions. After iterating through all the virus particles, the update method returns the number of virus particles in the patient at the end of the time step.

Hint: mutating objects

Note that the mapping between time steps and actual time will vary depending on the type of virus being considered, but for this problem set, think of a time step as a simulated hour of time.

About the grader: When defining a Patient class member variable to store the viruses list representing the virus population, please use the name self.viruses in order for your code to be compatible with the grader and to pass one of the tests.
