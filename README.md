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
		String name = "postfix.txt";
		Calculadora calc = new Calculadora(name);
	}

	}
	
	
Calculadora:
	public class Calculadora{
	private File txt;
	private char[] instruccion;
	private StackVector<Integer> pila;
	int resultado = 0;
	
	public Calculadora(String nombre){
		instruccion = new char[13];
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
	
	public void operar(){
		
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
				default:
					pila.push(Character.getNumericValue(instruccion[i]));
					break;
			}
		}
	}
	
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
