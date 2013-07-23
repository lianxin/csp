import java.util.ArrayList;
import java.util.Random;
//for txt io
import java.io.FileReader;
import java.io.BufferedReader;
import java.io.IOException;
import java.util.Scanner;
import java.util.Locale;

public class nae {
  private static class naeClause{
		//nae3sat clause takes three variables
		//integers x,y and z are the indexes of the variables
		int x;
		int y;
		int z;
		
		naeClause(int x,int y,int z){
			this.x=x;
			this.y=y;
			this.z=z;
		}
	}
	
	public static boolean isTrueClause(naeClause clause,boolean[] sol){
		//evaluate an nae3sat clause
		boolean a,b,c;
		a=sol[Math.abs(clause.x)];
		b=sol[Math.abs(clause.y)];
		c=sol[Math.abs(clause.z)];
		
		if(clause.x<0) a=a?false:true;
		if(clause.y<0) b=b?false:true;
		if(clause.z<0) c=c?false:true;
		if(a==b && a==c) return false;
		else return true;
	}
	
	public static ArrayList<naeClause> genProblem(int n_var, int n_cls){
		//randomly generate an instance of nae3sat, given the number of
		//variables and clauses
		//variables may be negated, x<0 means the xth variable is negated
		ArrayList<naeClause> clauses=new ArrayList<naeClause>();
		Random rd=new Random();
		int i;
		int x,y,z;
		for(i=0;i<n_cls;i++){
			x=rd.nextInt(n_var)+1;
			y=rd.nextInt(n_var)+1;
			z=rd.nextInt(n_var)+1;
			x=rd.nextBoolean()?x:-x;
			y=rd.nextBoolean()?y:-y;
			z=rd.nextBoolean()?z:-z;
			naeClause clause=new naeClause(x,y,z);
			clauses.add(clause);
		}
		return clauses;
	}
	
	public static void printCls(ArrayList<naeClause> clauses){
		//print the clauses
		System.out.println("Clauses:");
		for(naeClause i:clauses){
			System.out.print("nae(");
			System.out.print(i.x);System.out.print(',');
			System.out.print(i.y);System.out.print(',');
			System.out.print(i.z);System.out.print(')');
			System.out.println();
		}
	}
	
	public static void printVar(boolean[] sol){
		//print the solution (the set of variables which make the nae instance true)
		int i;
		System.out.println("Solution:");
		for(i=1;i<sol.length;i++){
			System.out.print(sol[i]);System.out.print(' ');
		}
	}
	
	public static boolean findSolution(ArrayList<naeClause> clauses,boolean[] sol,int n_var,int n_cls){
		//find the solution using Moser&Tardos method
		boolean isSat=false;
		Random rd=new Random();
		int timeout=n_cls*3*100; //the max steps allowed
		int i;
		int unsatc;
		//first set all variables randomly
		for(i=0;i<n_var+1;i++) sol[i]=rd.nextBoolean();
		//repeat until all clauses are satisfied
		while(!isSat){
			isSat=true;
			unsatc=-1;
			//pick an unsatisfied clause
			for(i=0;i<n_cls;i++){
				if(!isTrueClause(clauses.get(i),sol)) {
					isSat=false;
					unsatc=i;
					break;
				}
			}
			//randomly reset all variables in that unsatisfied clause
			if(unsatc!=-1){
				sol[Math.abs(clauses.get(unsatc).x)]=rd.nextBoolean();
				sol[Math.abs(clauses.get(unsatc).y)]=rd.nextBoolean();
				sol[Math.abs(clauses.get(unsatc).z)]=rd.nextBoolean();
			}
			timeout--;
			//if the maximum of steps is exceeded, 
			//stop the process and return an failure of finding solution
			if(timeout<0) break;  
		}
		return isSat;
	}
	
	public static void main(String[] args) throws IOException{
		//There are 8 variables and 5 clauses by default, if user inputs no arguments
		int n_var=8;
		int n_cls=5;
		ArrayList<naeClause> clauses=new ArrayList<naeClause>();
		
		//User inputs 2 arguments: number of variables, number of clauses
		if(args.length==2){
			try {
				    n_var = Integer.parseInt(args[0]);
				    n_cls = Integer.parseInt(args[1]);
				} catch (NumberFormatException e) {
				     System.err.println("Arguments must be positive integers");
				     System.exit(1);
				}
		}
		
		//User inputs 1 argument: the filename of text file
		// the input text file contains the number of variables, the number of clauses,
		// and the clauses
		if(args.length==1){
			String filename=args[0];
			Scanner s = null;
	        int i;
	        int x,y,z;

	        try {
	            s = new Scanner(new BufferedReader(new FileReader(filename)));
	            s.useLocale(Locale.US);
	            
	            n_var=s.nextInt();
	            n_cls=s.nextInt();
	            for(i=0;i<n_cls;i++){
	            	x=s.nextInt();
	            	y=s.nextInt();
	            	z=s.nextInt();
	            	naeClause clause=new naeClause(x,y,z);
	            	clauses.add(clause);
	            }
	        } finally {
	            s.close();
	        }
		}
		
		//sol[1] to sol[n_var] are the variables
		boolean[] sol=new boolean[n_var+1];
		
		//generate an instance randomly
		if(args.length!=1) clauses=genProblem(n_var,n_cls);
		
		printCls(clauses);
		boolean isSat=findSolution(clauses,sol,n_var,n_cls);
		
		//if an solution is found, print "SAT" and the solution
		//otherwise the nae instance might be unsatisfiable, print "UNSAT"
		if(isSat) {
			System.out.println("SAT");
			printVar(sol);
		}
		else System.out.println("UNSAT");

	}

}
