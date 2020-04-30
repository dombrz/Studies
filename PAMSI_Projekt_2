#include<iostream>
#include<fstream>
#include <stdlib.h>
#include <ctime>
#include <conio.h>
using namespace std;



//Klasa rKRAWÊD¯
class Edge
{
public:
	int start, end, weight;
};

//Klasa LISTA
class ListGraph
{
public:
	int amount_V, amount_E, start_Node;
	Edge *tab_edge;
	void ReadFromFile();
	void SaveFile();
	void View();
	void FillGraph(int m_V, int density);
	void delete_graph();
	int ShowWeight(int start, int end);
	bool CheckIfExist(int start, int end, int amount_elements);
};


//KLASA MACIERZ
class MatrixGraph
{
    public:
    int amount_V, amount_E, start_Node;
    int **matrix;
    void ReadFromFile();
	void SaveFile();
	void View();
	void FillGraph(int m_V, int density);
	void delete_graph();
	int ShowWeight(int start, int end);
};


class Dijkstry
{
    public:
    int *tab_used;
    int *tab_path_length;
    int **tab_path;
    int how_many_used;
    int amount_V;
    void paths(ListGraph graph, int start, int searched, bool if_one, bool view, bool save);
    void paths(MatrixGraph graph, int start, int searched, bool if_one, bool view, bool save);
    int used(int elem);
    int how_manyused();
};






/////////////////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////  LISTA  ////////////////////////////////////////////////
/////////////////////////////////////////////////////////////////////////////////////////////

void ListGraph::ReadFromFile()    //Wczytanie grafu z pliku Input.txt
{
	fstream file;
	file.open("Input.txt", ios::in);
	if (file.good() == false)
	{
		cout<<"BLAD ODCZYTU!";

	}

	file >> amount_E >> amount_V >> start_Node;
	tab_edge = new Edge[amount_E];

	for (int i = 0; i < amount_E; i++)
	{
		file >> tab_edge[i].start >> tab_edge[i].end >> tab_edge[i].weight;
	}

	file.close();
}

void ListGraph::SaveFile()  //Zapis grafu do pliku Output.txt
{
	fstream file;
	file.open("Output.txt", ios::out);


	file<< amount_E << " " << amount_V << " " << start_Node<< endl;

	for (int i = 0; i < amount_E; i++)
	{
		file << tab_edge[i].start<< " " << tab_edge[i].end<< " " << tab_edge[i].weight<< endl;
	}

	file.close();
}

void ListGraph::View()    //Wyœwietl graf
{
    for (int i = 0; i < amount_E; i++)
    {
        cout << tab_edge[i].start << " " << tab_edge[i].end << " " << tab_edge[i].weight << endl;
    }
}




bool ListGraph::CheckIfExist(int beg, int en, int amount_elem)     //SprawdŸ czy krawedŸ istnieje (aby unikn¹æ tworzenia kilku krawêdzi pomiêdzy tymi samymi wierzcho³kami w czasie genereacji losowego grafu
{
	for (int i = 0; i < amount_elem; i++)
	{
		if ((tab_edge[i].start == beg && tab_edge[i].end == en) || (tab_edge[i].start == en && tab_edge[i].end == beg)) return true;
	}
	return false;
}



int infinity=100000000;

void ListGraph::FillGraph(int m_V, int density)   //Generuj losowy graf
{
	amount_V = m_V;
	amount_E = 0;

	for (int i = amount_V - 1; i > 0; i--)
	{
		amount_E += i;                          // Obliczenie liczby krawêdzi na podstawie podanej gêstoœci (density)
	}

	amount_E = amount_E * (density / 100.0);
	tab_edge = new Edge[amount_E];


	for (int i = 0; i < amount_E; i++)
	{
		tab_edge[i].start = infinity;
		tab_edge[i].end = infinity;
		tab_edge[i].weight= infinity;
	}


	if (density == 100)    // Przypadek grafu pe³nego
	{
		int edge_number = 0;
		for (int i = 0; i < amount_V; i++)
		{
			for (int j = 0; j < amount_V; j++)
			{
				if (i < j)
				{
					tab_edge[edge_number].start = i;
					tab_edge[edge_number].end = j;
					tab_edge[edge_number].weight = rand() % 30 + 1;
					edge_number++;
				}
			}
		}
	}
	else
	{
		for (int i = 0; i < amount_E; i++)      //Dla grafów niepe³nych
		{
			int begin_ed = rand() % amount_V;
            int end_ed = rand() % amount_V;
			if (begin_ed != end_ed)
			{
				if (!CheckIfExist(begin_ed, end_ed, i + 1))
				{
					tab_edge[i].start = begin_ed;
					tab_edge[i].end= end_ed;
					tab_edge[i].weight = rand() % 30 + 1;
				}
				else i--;
			}
			else i--;
		}
	}
}


void ListGraph::delete_graph()     //usuñ graf
{
	delete tab_edge;
}


int ListGraph::ShowWeight(int start, int end)      //Zwróc wagê krawêdzi
{
	for (int i = 0; i < amount_E; i++)
	{
		if ((tab_edge[i].start == start) && (tab_edge[i].end == end)) return tab_edge[i].weight;
		else if ((tab_edge[i].start == end) && (tab_edge[i].end == start)) return tab_edge[i].weight;
	}
	return infinity;
}







//////////////////////////////////////////////////////////////////////////////////////////////////////
/////////////////////////////////////////  MACIERZ  //////////////////////////////////////////////////
//////////////////////////////////////////////////////////////////////////////////////////////////////


void MatrixGraph::ReadFromFile()        //Wczytanie grafu z pliku Input.txt
{
	fstream file;
	file.open("Input.txt", ios::in);
	if (file.good() == false)
	{
		cout<< "BLAD ODCZYTU!";
		exit(0);
	}

	file >> amount_E >> amount_V >> start_Node;
	matrix = new int*[amount_V];

	for (int i = 0; i < amount_V; i++)
	{
		matrix[i] = new int[amount_V];
		for (int j = 0; j < amount_V; j++)
		{
			if (i == j) matrix[i][j] = 0;
			else matrix[i][j] = infinity;
		}
	}

	int start, end, weight;

	for (int i = 0; i < amount_E; i++)
	{
		file >> start >> end >> weight;
		matrix[start][end] = weight;
		matrix[end][start] = weight;
	}

	file.close();
}



void MatrixGraph::SaveFile()     //Zapis grafu do pliku Output.txt
{
	fstream file;
	file.open("Output.txt", ios::out);
	start_Node = 0;
	while (start_Node < 0)
	{
		cout << "Podaj wierzcholek startowy z przedzialu od 0 do " << amount_V - 1 << " :";
		cin >> start_Node;
	}


	file << amount_E << " " << amount_V << " " << start_Node << endl;

	for (int i = 0; i < amount_V; i++)
	{
		for (int j = 0; j < amount_V; j++)
		{
			if (matrix[i][j] != 0 && matrix[i][j] != infinity && i <= j)
			{
				file << i << " " << j << " " << matrix[i][j] << endl;
			}
		}
	}

	file.close();
}


void MatrixGraph::View()   //Wyœwietl graf
{

    cout<<endl;
    cout<<"     ";
    for(int k=0; k<amount_V;k++)
    {
        if (k<10) cout<<k<<"  ";
        if (k>=10) cout<<k<<" ";
    }
    cout<<endl;
    cout<<"    ";
    for(int h=0; h<amount_V;h++)
    {
        cout<<"___";
    }

     cout<<endl;
	for (int i = 0; i < amount_V; i++)
	{
	    cout<<"   |"<<endl;
	   if (i<10) cout<<" "<<i<<" | ";
	   if(i>=10) cout<<i<<" | ";
		for (int j = 0; j < amount_V; j++)
		{

			if (matrix[i][j] == infinity) cout << "0";
			else cout << matrix[i][j];


			if(matrix[i][j]<10 || matrix[i][j]==infinity )   cout <<"  ";

            if(matrix[i][j]>=10 && matrix[i][j]!=infinity) cout <<" ";




        }
    cout << endl;
	}

	cout<< endl<<"0 - brak krawedzi pomiedzy wierzcholkami"<<endl<<endl<<endl<<endl;
}

void MatrixGraph::FillGraph(int m_V, int density)
{
	amount_V = m_V;

	amount_E = 0;
	for (int i = amount_V - 1; i > 0; i--)
	{
		amount_E += i;
	}

	amount_E = amount_E * (density/ 100.0);

	matrix = new int*[amount_V];
	for (int i = 0; i < amount_V; i++)
	{
		matrix[i] = new int[amount_V];
		for (int j = 0; j < amount_V; j++)
		{
			if (i == j) matrix[i][j] = 0;
			else matrix[i][j] = infinity;
		}
	}

	if (density== 100)
	{
		for (int i = 0; i < amount_V; i++)
		{
			for (int j = 0; j < amount_V; j++)
			{
				if (i < j)
				{
					int weightt = rand() % 30 + 1;
					matrix[i][j] = weightt;
					matrix[j][i] = weightt;
				}
			}
		}
	}
	else
	{
		for (int i = amount_E; i > 0; i--)
		{
			int row = rand() % amount_V, columne = rand() % amount_V;
			if (matrix[row][columne] == infinity && matrix[columne][row] == infinity && row != columne)  //Sprawdź czy nie istnieje już taka krawędź
			{
				int weighttt = rand() % 30 + 1;
				matrix[row][columne] = weighttt;
				matrix[columne][row] = weighttt;
			}
			else
			{
				i++;
			}
		}
	}
}


void MatrixGraph::delete_graph()
{
	for (int i = 0; i <amount_V; i++)
	{
		delete matrix[i];
	}
	delete matrix;
}


int MatrixGraph::ShowWeight(int start, int end)
{
	return matrix[start][end];
}



////////////////////////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////// DIJKSTRY/////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////


int Dijkstry::used(int elem)
{
	for (int i = 0; i < amount_V; i++)
	{
		if (tab_used[i] == elem) return i;
	}
	return -1;
}


int Dijkstry::how_manyused()
{
	for (int i = 0; i < amount_V; i++)
	{
		if (tab_used[i] == infinity) return i;
	}
	return amount_V;
}



////////////////////////////////DIJKSTRY - LISTA///////////////////////////////////////////////////////

void Dijkstry::paths(ListGraph graph, int start, int searched, bool if_one, bool view, bool save)

{

	how_many_used = 0;
	amount_V = graph.amount_V;
	tab_used = new int[amount_V];
	tab_path_length = new int[amount_V];
	tab_path = new int*[amount_V];
	for (int i = 0; i < amount_V; i++)
	{
		tab_used[i] = infinity;
		tab_path_length[i] = 0;
		tab_path[i] = new int[i + 1];
		for (int j = 0; j <= i; j++)
		{
			tab_path[i][j] = infinity;
		}

	}

	int length = infinity;
	int length_a = infinity;
	int length_b = infinity;
	int connect = infinity;
	int counter = 0;
	int option = 0;

	while (counter < amount_V)
	{
		length = infinity;
		length_a = infinity;
		length_b = infinity;
		for (int i = 0; i < amount_V; i++)
		{
			if (start != i && used(i) == -1)
			{
				if (length > graph.ShowWeight(start, i))
				{
					length = graph.ShowWeight(start, i);
					connect = i;
					option = -1;
				}
			}
		}
		for (int i = 0; i < amount_V; i++)
		{
			if (start != i && how_manyused() > 0 && used(i) > -1)
			{
				if ((tab_path_length[counter] + length) > (tab_path_length[used(i)] + graph.ShowWeight(connect, i)) && length_b > (tab_path_length[used(i)] + graph.ShowWeight(connect, i)))
				{
					length_a = graph.ShowWeight(connect, i);
					length_b = tab_path_length[used(i)] + graph.ShowWeight(connect, i);
					option = used(i);
				}
			}
		}

		if (option== -1)
		{
			tab_path[counter][counter] = start;
			for (int j = 0; j <= counter; j++)
			{
				if (counter + 1 < amount_V) tab_path[counter + 1][j] = tab_path[counter][j];
			}

			tab_path_length[counter + 1] = tab_path_length[counter ] + length;
			tab_used[counter ] = start;
			counter ++;
			start = connect;
		}
		else
		{
			tab_path[counter][counter] = start;
			for (int j = 0; j < option + 1; j++)
			{
				if (counter + 1 < amount_V) tab_path[counter + 1][j] = tab_path[option][j];
			}

			tab_path_length[counter + 1] = tab_path_length[option] + length_a;
			tab_used[counter] = start;
			counter++;
			if (connect != infinity) start = connect; else counter = amount_V;
		}
		if ((if_one == true && tab_used[counter - 1] == searched) || tab_used[counter - 1] == connect) { counter = amount_V; }
	}



	if (view == true)     //Wyświetlenie (opcjonalne)
	{
		cout << endl;

		int completed = how_manyused();
		for (int i = 0; i < completed; i++)
		{
			cout << "Sciezka: ";
			for (int j = 0; j <= i; j++)
			{
				if (tab_path[i][j] != infinity) cout<< "->" << tab_path[i][j];
			}
			cout << "        Koszt sciezki: ";
			cout << tab_path_length[i] << " ";
			cout << endl;
		}
		cout << endl;
	}

	if (save == true)  //Zapis do pliku (opcjonalny)
	{
		fstream file;
		file.open("Paths.txt", ios::out);
		int completed = how_manyused();
		for (int i = 0; i < completed; i++)
		{
			file << "Sciezka: ";
			for (int j = 0; j <= i; j++)
			{
				if (tab_path[i][j] != infinity) file << "->" << tab_path[i][j];
			}
			file << "   Koszt sciezki: ";
			file << tab_path_length[i] << " ";
			file << endl;
		}
		file << endl;
		file.close();
	}
}
////////////////////////////////////////////////////////////////////////////////////////////////////////////////






///////////////////////////////////////////DJIKSTRY MACIERZ/////////////////////////////////////////////////////


void Dijkstry::paths(MatrixGraph graph, int start, int searched, bool if_one, bool view, bool save)
{

	how_many_used = 0;
	amount_V = graph.amount_V;
	tab_used = new int[amount_V];
	tab_path_length = new int[amount_V];
	tab_path = new int*[amount_V];
	for (int i = 0; i < amount_V; i++)
	{
		tab_used[i] = infinity;
		tab_path_length[i] = 0;
		tab_path[i] = new int[i + 1];
		for (int j = 0; j <= i; j++)
		{
			tab_path[i][j] = infinity;
		}

	}

	int length = infinity;
	int length_a = infinity;
	int length_b = infinity;
	int connect = infinity;
	int counter = 0;
	int option = 0;


	while (counter < amount_V)
	{
		length = infinity;
		length_a = infinity;
		length_b = infinity;
		for (int i = 0; i < amount_V; i++)
		{
			if (start != i && used(i) == -1)
			{
				if (length > graph.matrix[start][i])
				{
					length = graph.matrix[start][i];
					connect = i;
					option = -1;
				}
			}
		}
		for (int i = 0; i < amount_V; i++)
		{
			if (start != i && how_manyused() > 0 && used(i) > -1)
			{
				if ((tab_path_length[counter] + length) > (tab_path_length[used(i)] + graph.matrix[connect][i]) && length_b > (tab_path_length[used(i)] + graph.matrix[connect][i]))
				{
					length_a = graph.matrix[connect][i];
					length_b = tab_path_length[used(i)] + graph.matrix[connect][i];
					option = used(i);
				}
			}
		}

		if (option == -1)
		{
			tab_path[counter][counter] = start;
			for (int j = 0; j <= counter; j++)
			{
				if (counter + 1 < amount_V) tab_path[counter + 1][j] = tab_path[counter][j];
			}

			tab_path_length[counter + 1] = tab_path_length[counter] + length;
			tab_used[counter] = start;
			counter++;
			start = connect;
		}
		else
		{
			tab_path[counter][counter] = start;
			for (int j = 0; j < option + 1; j++)
			{
				if (counter + 1 < amount_V) tab_path[counter + 1][j] = tab_path[option][j];
			}

			tab_path_length[counter + 1] = tab_path_length[option] + length_a;
			tab_used[counter] = start;
			counter++;
			if (connect != infinity) start = connect; else counter = amount_V;
		}
		if ((if_one == true && tab_used[counter - 1] == searched) || tab_used[counter - 1] == connect) { counter = amount_V; }

	}



	if (view == true)  //Wyświetlenie (opcjonalne)
	{


		int completed = how_manyused();
		for (int i = 0; i < completed; i++)
		{
			cout << "Sciezka: ";
			for (int j = 0; j <= i; j++)
			{
				if (tab_path[i][j] != infinity) cout<< "->" << tab_path[i][j];
			}
			cout << "   Koszt sciezki: ";
			cout << tab_path_length[i] << " ";
			cout << endl;
		}
		cout << endl;
	}






	if (save == true)    //Zapis do pliku (opcjonalny)
	{
		fstream file;
		file.open("Paths.txt", ios::out);
		int completed = how_manyused();
		for (int i = 0; i < completed; i++)
		{
			file << "Sciezka: ";
			for (int j = 0; j <= i; j++)
			{
				if (tab_path[i][j] != infinity) file << tab_path[i][j] << " ";
			}
			file << "   Koszt sciezki: ";
			file << tab_path_length[i] << " ";
			file << endl;
		}
		file << endl;
		file.close();
	}
}













int main()
{
    clock_t start, stop;
    double time_single = 0.0, time_sum = 0.0;
    srand(time(NULL));
    char choice, choice2, choice3;
    char choice_save, choice_read, choice_view;
    int V_number, chose_density;
    bool if_view, if_save;
    Dijkstry search_path;
     cout << "Wybierz : " << endl;
	 cout << "1. Dzialania na pojedynczym grafie" << endl;
	 cout << "2. Badanie wydajnosci na 100 losowych grafach" << endl;
     choice = _getch();
	 system("cls");
	 switch (choice)

     {
     case '1':
        cout<<"Wybierz:"<<endl;
        cout<<"1.Wczytaj graf z pliku"<<endl;
        cout<<"2.Wygeneruj losowy graf"<<endl;
        choice2=getch();
        system("cls");
        switch(choice2)
        {
        case '1':
        cout<<"Wybierz postac grafu:"<<endl;
        cout<<"1.Lista sasiedztwa"<<endl;
        cout<<"2.Macierz"<<endl;
        choice3=getch();
        system("cls");

        switch(choice3)
        {
        case '1':
            ListGraph graph_l;
            graph_l.ReadFromFile();
            cout<<"Zapisano graf w postaci listy"<<endl;
            cout<<"Czy chcesz zapisac graf do pliku Output.txt?"<<endl;
            cout<<"1.Tak"<<endl;
            cout<<"2.Nie"<<endl;
            choice_save=getch();
            system("cls");
            cout<<"Czy chcesz wyswietlic graf na ekranie?"<<endl;
            cout<<"1.Tak"<<endl;
            cout<<"2.Nie"<<endl;
            choice_view=getch();
            system("cls");
            if (choice_view=='1') graph_l.View();
            if (choice_save=='1') graph_l.SaveFile();
            search_path.paths(graph_l,0,0,false,true,true);
            cout<<"Zapisano sciezki w pliku Paths.txt:"<<endl;
            break;
        case '2':
            MatrixGraph graph_m;
            graph_m.ReadFromFile();
            cout<<"Zapisano graf w postaci macierzy"<<endl;
            cout<<"Czy chcesz zapisac graf do pliku Output.txt?"<<endl;
            cout<<"1.Tak"<<endl;
            cout<<"2.Nie"<<endl;
            choice_save=getch();
            system("cls");
            cout<<"Czy chcesz wyswietlic graf na ekranie?"<<endl;
            cout<<"1.Tak"<<endl;
            cout<<"2.Nie"<<endl;
            choice_view=getch();
            system("cls");
            if (choice_view=='1') graph_m.View();
            if (choice_save=='1') graph_m.SaveFile();
            search_path.paths(graph_m,0,0,false,true,true);
            cout<<"Zapisano sciezki w pliku Paths.txt:"<<endl;
            break;
        }
        case '2':
        cout<<"Wybierz postac grafu:"<<endl;
        cout<<"1.Lista sasiedztwa"<<endl;
        cout<<"2.Macierz"<<endl;
        choice3=getch();
        system("cls");

        switch(choice3)
        {
        case '1':
            cout<<"Podaj liczbe wierzcholkow:"<<endl;
            cin>>V_number;
            cout<<endl;
            cout<<"Podaj % gestosci (od 1 do 100):"<<endl;
            cin>>chose_density;
            ListGraph graph_random_l;
            graph_random_l.FillGraph(V_number,chose_density);
            cout<<"Wygenerowano graf w postaci listy"<<endl;
            cout<<"Czy chcesz zapisac graf do pliku Output.txt?"<<endl;
            cout<<"1.Tak"<<endl;
            cout<<"2.Nie"<<endl;
            choice_save=getch();
            system("cls");
            cout<<"Czy chcesz wyswietlic graf na ekranie?"<<endl;
            cout<<"1.Tak"<<endl;
            cout<<"2.Nie"<<endl;
            choice_view=getch();
            system("cls");
            if (choice_view=='1') graph_random_l.View();
            if (choice_save=='1') graph_random_l.SaveFile();
            search_path.paths(graph_random_l,0,0,false,true,true);
            cout<<"Zapisano sciezki w pliku Paths.txt:"<<endl;
            break;

        case '2':
            cout<<"Podaj liczbe wierzcholkow:"<<endl;
            cin>>V_number;
            cout<<endl;
            cout<<"Podaj % gestosci (od 1 do 100):"<<endl;
            cin>>chose_density;
            MatrixGraph graph_random_m;
            graph_random_m.FillGraph(V_number,chose_density);
            cout<<"Wygenerowano graf w postaci listy"<<endl;
            cout<<"Czy chcesz zapisac graf do pliku Output.txt?"<<endl;
            cout<<"1.Tak"<<endl;
            cout<<"2.Nie"<<endl;
            choice_save=getch();
            system("cls");
            cout<<"Czy chcesz wyswietlic graf na ekranie?"<<endl;
            cout<<"1.Tak"<<endl;
            cout<<"2.Nie"<<endl;
            choice_view=getch();
            system("cls");
            if (choice_view=='1') graph_random_m.View();
            if (choice_save=='1') graph_random_m.SaveFile();
            search_path.paths(graph_random_m,0,0,false,true,true);
            cout<<"Zapisano sciezki w pliku Paths.txt:"<<endl;
            break;

        }



        }

        case '2':
        cout<<"Wybierz postac grafu:"<<endl;
        cout<<"1.Lista sasiedztwa"<<endl;
        cout<<"2.Macierz"<<endl;
        choice3=getch();
        system("cls");
        switch(choice3)
        {
        case '1':
            cout<<"Podaj liczbe wierzcholkow:"<<endl;
            cin>>V_number;
            cout<<endl;
            cout<<"Podaj % gestosci (od 1 do 100):"<<endl;
            cin>>chose_density;
            cout<<endl;
            ListGraph graph_l_efficiency;
           for(int i=0; i<100; i++)
           {
	        graph_l_efficiency.FillGraph(V_number,chose_density);
            start = clock();
            search_path.paths(graph_l_efficiency, 0, 0, false, false, false);
            stop = clock();
            time_single = (double(stop-start)/CLOCKS_PER_SEC);
	        time_sum=time_sum+time_single;
            graph_l_efficiency.delete_graph();
          }

	cout<<"Sredni czas dzialania algorytmu dla listy sasiedztwa:"<<time_sum/100<<" sek "<<endl;
    break;


           case '2':
            cout<<"Podaj liczbe wierzcholkow:"<<endl;
            cin>>V_number;
            cout<<endl;
            cout<<"Podaj % gestosci (od 1 do 100):"<<endl;
            cin>>chose_density;
            cout<<endl;
            MatrixGraph graph_m_efficiency;
           for(int i=0; i<100; i++)
           {
	        graph_m_efficiency.FillGraph(V_number,chose_density);
            start = clock();
            search_path.paths(graph_m_efficiency, 0, 0, false, false, false);
            stop = clock();
            time_single = (double(stop-start)/CLOCKS_PER_SEC);
	        time_sum=time_sum+time_single;
            graph_m_efficiency.delete_graph();
          }

	cout<<"Sredni czas dzialania algorytmu dla macierzy:"<<time_sum/100<<" sek "<<endl;
    break;


     }

     }

}
