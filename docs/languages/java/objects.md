---
tags: [java, class, object, lombok]
---
# OBJECTS

**TO BE TRANSLATED INTO ENGLISH**

Tipi di classi
- logica  (service, controller, component, EJB, helper....)
- dati    (dto, vo, entity, beans,...)
- utility (funzioni statiche)


Categorie di classi di dato


mutabili		@Data		Da evitare
JavaBeans		@Data		Solo quando richiesto dal contesto (JPA, beans Vaadin/JSF). Implica mutabilità
immutabili		@Value		Soluzione standard


Come creare una classe di logica

METODO			COME			QUANDO

new			Operatore new		Per le dipendenzi interne (classi di supporto, helper,..)

dependency injection	@EJB (EJB)              Da preferire per le dipendenze esterne (la D di SOLID)
                        @Inject (CDI)
                        @Autowire (Spring)

lookup                  JEE (Initial)Context
                        Spring

abstract factory	codice			Diverse implementazioni/varianti di una stessa interfaccia (Logger logger = LoggerFactory.getLogger("")
			qualificatori

singleton		enum			La classe svolge una funzione che deve essere univoca (scheduler) (Bloch #3)
 			@Singleton

utility			static final, costruttore privato	Da usare solo per funzioni senza stato (pure), Bloch #4 




Come creare una classe di dati


METODO			COME				QUANDO

new			operatore new			Da usare solo per JavaBeans (Bloch #1)

factory method		metodo static (es. "of")	Da preferire, fino a 2/3 campi  (Bloch #1)

builder			@Builder (lombok)		Da preferire, oltre i 2/3 campi  (Bloch #2)

abstract factory	codice				Diverse implementazioni/varianti di una stessa interfaccia

singleton		enum				Da evitare

prototype		clone				In genere da evitare




Altri modi per creare oggetti

<Clazz>.newInstance()
Serializzando - deserializzando
<Clazz>.getConstructore().newInstance()
