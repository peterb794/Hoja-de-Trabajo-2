Hoja-de-Trabajo-2
=================
Stack:
  
    public interface Stack<E> 
    {
  
     public void push(E item);
     // pre: 
     // post: item is added to stack
     // will be popped next if no intervening push
     
     public E pop();
     // pre: stack is not empty
     // post: most recently pushed item is removed and returned
     
     public E peek();
     // pre: stack is not empty
     // post: top value (next to be popped) is returned
     
     public boolean empty();
     // post: returns true if and only if the stack is empty
     
     public int size();
     // post: returns the number of elements in the stack
  
    }
    
StackArrayList:

    public class StackArrayList<E> implements Stack<E>
    import java.util.ArrayList;
    {
	  protected ArrayList<E> data;

  	public StackArrayList()
  	// post: constructs a new, empty stack
  	{
  		data = new ArrayList<E>();
  	}
  
  	public void push(E item)
  	// post: the value is added to the stack
  	//          will be popped next if no intervening push
  	{
  		data.add(item);
  	}
  
  	public E pop()
  	// pre: stack is not empty
  	// post: most recently pushed item is removed and returned
  	{
  		return data.remove(size()-1);
  	}
  
  	public E peek()
  	// pre: stack is not empty
  	// post: top value (next to be popped) is returned
  	{
  		return data.get(size() - 1);
  	}
  	
  	public int size()
  	// post: returns the number of elements in the stack
  	{
  		return data.size();
  	}
    
  	public boolean empty()
  	// post: returns true if and only if the stack is empty
  	{
  		return size() == 0;
  	}
    }
    
Stack Vector:
    import java.util.Vector;
    
    
    public class StackVector<E> implements Stack<E> {

	protected Vector<E> data;
	
	public StackVector(){
		// post: constructs a new, empty stack
		data = new Vector<E>(1,1);
	}
	
	@Override
	public void push(E item) {
		// post: the value is added to the stack
		//          will be popped next if no intervening push
		data.add(item);
	}

	@Override
	public E pop() {
		// pre: stack is not empty
		// post: most recently pushed item is removed and returned
		return data.remove(size()-1);
	}

	@Override
	public E peek() {
		// pre: stack is not empty
		// post: top value (next to be popped) is returned
		return data.get(size() - 1);
	}

	@Override
	public boolean empty() {
		// post: returns true if and only if the stack is empty
		return data.isEmpty();
	}

	@Override
	public int size() {
		// post: returns the number of elements in the stack
		return data.size();
	}

	}

Driver:

	public static void main(String[] args) {
		//Creacion de calculadora con archivo provisto.
		String name = "postfix.txt";
		Calculadora calc = new Calculadora(name);
		//Llevar a cabo lectura y operacion de instrucciones.
		calc.operar();
		//Imprimir instrucciones para verificacion de lectura.
		System.out.println("Instrucciones: ");
		for(int i = 0; i < calc.getInstruccion().length; i++){
			System.out.println(calc.getInstruccion()[i]);
		}
		//Despligue de resultado final.
		System.out.println("\n\n -------Resultado: "+ calc.getResultado());
	}

	}
	
	
Calculadora:

public class Calculadora {
	private File txt;					//Archivo con instrucciones aritmeticas.
	private char[] instruccion;			//Array para almanacenar cada linea como una instruccion.
	private StackVector<Integer> pila;	//Implementacion con funcionalidad de pila.
	int resultado = 0;					//Utilizado para almacenar resultado de cada linea.
	
	//Metodo constructor. Recibe como parametro el nombre del archivo por leer. 
	public Calculadora(String nombre){
		//Valores iniciales para atributos.
		instruccion = new char[13];
		//Creacion de pila de enteros utilizando genericos.
		pila = new StackVector<Integer>();
		pila.push(resultado);
		//Creacion del archivo para lectura.
		try{
			txt = new File(nombre);
		}
		//Excepcion que se lanza cuando el archivo no existe.
		catch(NullPointerException e){
			JOptionPane.showMessageDialog(null, "No existe el archivo.", "ERROR", 
					JOptionPane.ERROR_MESSAGE);
			System.exit(0);
		}
	    try{
	    	//Creacion de scanner especializado para el archivo.
	    	Scanner s = new Scanner(txt);
			//Lee todo el archivo; almacena en la matriz. 
	        while (s.hasNextLine()) {
	            instruccion = s.nextLine().toCharArray();
	        }
	        //Se cierra el Scanner para evitar "leaks".
	        s.close();
	    } 
	    //Excepcion que se lanza cuando hay un error en lectura.
	    catch (FileNotFoundException e) {
			JOptionPane.showMessageDialog(null, "Archivo no encontrado.", "ERROR", 
					JOptionPane.ERROR_MESSAGE);
			System.exit(0);
	    }
	}
	
	//Metodo para llevar a cabo instrucciones. Utiliza metodos de apoyo.
	public void operar(){
		//Evalua la linea obtenida del archivo texto. 
		for(int i = 0; i < instruccion.length; i++){
			switch(instruccion[i]){
				case('+'):
					suma();
					break;
				case('*'):
					multiplicacion();
					break;
				case('/'):
					division();
					break;
				case('-'):
					resta();
					break;
				case(' '):
					break;
				//Para cada numero.
				default:
					pila.push(Character.getNumericValue(instruccion[i]));
					break;
			}
		}
	}
	
	//Metodos de apoyo para operaciones.
	public void suma(){
		resultado = pila.pop() + pila.pop();
		pila.push(resultado);
	}
	
	public void multiplicacion(){
		resultado = pila.pop() * pila.pop();
		pila.push(resultado);
	}
	
	public void division(){
		resultado = (1/pila.pop()) * pila.pop();
		pila.push(resultado);
	}
	
	public void resta(){
		resultado = -(pila.pop()) + pila.pop();
		pila.push(resultado);
	}
	
	//Metodos set y get para cada atributo.
	public StackVector<Integer> getPila() {
		return pila;
	}

	public void setPila(StackVector<Integer> pila) {
		this.pila = pila;
	}

	public int getResultado() {
		return resultado;
	}

	public void setResultado(int resultado) {
		this.resultado = resultado;
	}

	public File getTxt() {
		return txt;
	}

	public void setTxt(File txt) {
		this.txt = txt;
	}

	public char[] getInstruccion() {
		return instruccion;
	}

	public void setInstruccion(char[] instruccion) {
		this.instruccion = instruccion;
	}
}

StackVectorTest:
	public class StackVectorTest {
	
		@Test
		public final void testPush() {
			System.out.println("Probando Push\n");
			int item = 24;
			StackVector<Integer> instance = new StackVector<Integer>();
			instance.push(item);
			int resultado = instance.pop();
	        assertEquals(item, resultado);
			//fail("Not yet implemented"); // TODO
		}
	
		@Test
		public final void testPop() {
			System.out.println("Probando Pop\n");
			int item = 24;
			int item_2 = 12;
			StackVector<Integer> instance = new StackVector<Integer>();
			instance.push(item);
			instance.push(item_2);
			int resultado = instance.pop();
			resultado = instance.pop();
			assertEquals(item, resultado);
			//fail("Not yet implemented"); // TODO
		}
	
		@Test
		public final void testPeek() {
			System.out.println("Probando Peek\n");
			int item = 24;
			StackVector<Integer> instance = new StackVector<Integer>();
			instance.push(item);
			int resultado = instance.peek();
	        assertEquals(item, resultado);
			//fail("Not yet implemented"); // TODO
		}
	
		@Test
		public final void testEmpty() {
			System.out.println("Probando Empty\n");
			int item = 24;
			StackVector<Integer> instance = new StackVector<Integer>();
			instance.push(item);
			boolean expected = false;
			boolean resultado = instance.empty();
			assertEquals(expected, resultado);
			//fail("Not yet implemented"); // TODO
		}
	
		@Test
		public final void testSize() {
			System.out.println("Probando Push\n");
			int item = 24;
			StackVector<Integer> instance = new StackVector<Integer>();
			instance.push(item);
			int expected = 1;
			int resultado = instance.size();
	        assertEquals(expected, resultado);
			//fail("Not yet implemented"); // TODO
		}
	
	}
