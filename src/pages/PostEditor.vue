<script setup lang="ts">
import {
  AccessibilityHelp,
  Alignment,
  Autoformat,
  AutoLink,
  Autosave,
  BalloonToolbar,
  Base64UploadAdapter,
  Bold,
  ButtonView,
  Code,
  CodeBlock,
  DecoupledEditor,
  Editor,
  EditorConfig,
  Essentials,
  FindAndReplace,
  FontBackgroundColor,
  FontColor,
  FontFamily,
  FontSize,
  GeneralHtmlSupport,
  Heading,
  Highlight,
  HorizontalLine,
  HtmlComment,
  HtmlEmbed,
  Image,
  ImageInsert,
  ImageResize,
  ImageToolbar,
  Indent,
  IndentBlock,
  Italic,
  Link,
  Paragraph,
  RemoveFormat,
  RestrictedEditingMode,
  SelectAll,
  SpecialCharacters,
  SpecialCharactersArrows,
  SpecialCharactersCurrency,
  SpecialCharactersEssentials,
  SpecialCharactersLatin,
  SpecialCharactersMathematical,
  SpecialCharactersText,
  StandardEditingMode,
  Strikethrough,
  Subscript,
  Superscript,
  Table,
  TableCaption,
  TableCellProperties,
  TableColumnResize,
  TableProperties,
  TableToolbar,
  TextPartLanguage,
  TextTransformation,
  Underline,
  Undo,
} from "ckeditor5";

import "ckeditor5/ckeditor5.css";
import {onMounted, ref, toRefs, watch} from "vue";
//import {PdfManipulate} from "src/helpers/DocumentManipulator";
//import PdfView from "src/shared/components/PdfView/PdfView.vue";
import FontSizeModal from "src/modals/FontSizeModal.vue";
//import {useDocumentStore} from "src/stores/documentStore";
//import collaboration from "./Collaboration.vue";
//import {sgdeaAxios} from "src/services";
//import {useAuthStore} from "stores/auth.store";
import {useRoute} from "vue-router";

let desciption = ref<string>("");
const showFontSizeModal = ref(false);
const acceptedId = ref<number | null>(null);
const fontSize = ref<number>(11);
const filteredCollaborators =ref<Array<any>>([]);
//const documentStore = useDocumentStore();
//const auth = useAuthStore();
const route = useRoute();
let isInitialLoad = true;
const props = defineProps({
  base64: {
    type: String,
    default: ""
  },
  restric: {
    type: Boolean,
    default: false,
  },
  referentKey: {
    require: true,
    type: Object,
  },
  data: {
    require: true,
    type: Object,
  },
  document: {
    type: String,
    default: ""
  },
  revisado: {
    type: Boolean,
    required: false,
  },
  aprobado: {
    type: Boolean,
    required: false,
  },
  dataHeader: {
    require: false,
    type: Object,
  },
  isEditorEmpty: {
    require: false,
    type: Boolean,
  },
  imgBase64: {
    type: Object,
    required: false,
  },
  aling: false,
  collaboration: {
    type: Boolean,
    default: false,
  },
  changesCollaborators: {
    type: Array,
    default: [],
  },
  isCloner: {
    type: Boolean,
    default: false,
  }
});
const { revisado, aprobado } = toRefs(props);
const config: EditorConfig = {
  toolbar: {
    items: [
      "undo",
      "redo",
      "|",
      "heading",
      "|",
      "fontSize",
      "fontFamily",
      "fontColor",
      "fontBackgroundColor",
      "|",
      "bold",
      "italic",
      "underline",
      "|",
      "link",
      "insertTable",
      "highlight",
      // "blockquote",
      "codeBlock",
      "|",
      "alignment",
      "|",
      "outdent",
      "indent",
      "|",
      "insertImage",
      "|",
      // "verPdf",
      "tamañoFuente",
      "listaDesc",
      //props.restric ? "restrictedEditingException" : "",
    ],
  },
  plugins: [
    AccessibilityHelp,
    Alignment,
    Autoformat,
    AutoLink,
    Autosave,
    BalloonToolbar,
    Bold,
    Code,
    CodeBlock,
    Essentials,
    FindAndReplace,
    FontBackgroundColor,
    FontColor,
    FontFamily,
    FontSize,
    GeneralHtmlSupport,
    Heading,
    Highlight,
    HorizontalLine,
    HtmlComment,
    HtmlEmbed,
    Indent,
    IndentBlock,
    Italic,
    Link,
    Paragraph,
    RemoveFormat,
    SelectAll,
    SpecialCharacters,
    SpecialCharactersArrows,
    SpecialCharactersCurrency,
    SpecialCharactersEssentials,
    SpecialCharactersLatin,
    SpecialCharactersMathematical,
    SpecialCharactersText,
    Strikethrough,
    Subscript,
    Superscript,
    Table,
    TableCaption,
    TableCellProperties,
    TableColumnResize,
    TableProperties,
    TableToolbar,
    TextPartLanguage,
    TextTransformation,
    Underline,
    Undo,
    Image,
    ImageToolbar, // Plugin para mostrar la barra de herramientas en la imagen
    ImageResize, // Plugin para permitir redimensionar la imagen
    ImageInsert, // Inserción de imagen
    Base64UploadAdapter,
    props.restric ? RestrictedEditingMode : StandardEditingMode,
  ],
  balloonToolbar: ["bold", "italic", "|", "link"],
  blockToolbar: [
    "fontSize",
    "fontColor",
    "fontBackgroundColor",
    "|",
    "bold",
    "italic",
    "|",
    "link",
    "insertTable",
    "|",
    "outdent",
    "indent",
  ],
  fontFamily: {
    supportAllValues: true,
  },
  fontSize: {
    options: [6, 8, 10, 12, 14, 16, 18, "default", 20, 22, 26, 36],
    supportAllValues: true,
  },
  heading: {
    options: [
      {
        model: "paragraph",
        title: "Paragraph",
        class: "ck-heading_paragraph",
      },
      {
        model: "heading1",
        view: "h1",
        title: "Heading 1",
        class: "ck-heading_heading1",
      },
      {
        model: "heading2",
        view: "h2",
        title: "Heading 2",
        class: "ck-heading_heading2",
      },
      {
        model: "heading3",
        view: "h3",
        title: "Heading 3",
        class: "ck-heading_heading3",
      },
      {
        model: "heading4",
        view: "h4",
        title: "Heading 4",
        class: "ck-heading_heading4",
      },
      {
        model: "heading5",
        view: "h5",
        title: "Heading 5",
        class: "ck-heading_heading5",
      },
      {
        model: "heading6",
        view: "h6",
        title: "Heading 6",
        class: "ck-heading_heading6",
      },
    ],
  },
  htmlSupport: {
    allow: [
      {
        name: /^.*$/,
        styles: true,
        attributes: true,
        classes: true,
      },
    ],
  },
  link: {
    addTargetToExternalLinks: true,
    defaultProtocol: "https://",
    decorators: {
      toggleDownloadable: {
        mode: "manual",
        label: "Downloadable",
        attributes: {
          download: "file",
        },
      },
    },
  },
  menuBar: {
    isVisible: true,
  },
  table: {
    contentToolbar: [
      "tableColumn",
      "tableRow",
      "mergeTableCells",
      "tableProperties",
      "tableCellProperties",
    ],
  },
  restrictedEditing: props.restric
    ? {
        allowedAttributes: ["bold", "italic"],
        allowedCommands: ["bold", "italic"],
      }
    : undefined,
  autosave: {
    save: (data) => eventAutoSave(data),
  },
  image: {
    toolbar: [
      "toggleImageCaption",
      "imageTextAlternative",
      "ckboxImageEdit",
      "imageTextAlternative", // Alternar texto de la imagen
      "imageStyle:full",
      "imageStyle:side",
      "imageResize",
    ],
    resizeUnit: "%",
  },

  extraPlugins: [BloquearPlugin, FontSizePlugin, AddItemDescript],
};

const emit = defineEmits(["revisado", "aprobado", "datos-response"]);
const editorMenuBarElement = ref<HTMLElement | null>(null);
const editorToolbarElement = ref<HTMLElement | null>(null);
const isEdit = ref<Boolean>(true);
const editorRef = ref<Editor>();

onMounted(() => {
  filterCollaborations();
})

const filterCollaborations = () => {
  if (props.changesCollaborators && props.changesCollaborators.length > 0) {
    filteredCollaborators.value = props.changesCollaborators.filter(
      doc => doc.isProcesadoPorClonado === false && doc.procesoClonacionTerminado
    );
  }
}

function AddItemDescript(editor) {
  editor.ui.componentFactory.add("listaDesc", (locale) => {
    const button = new ButtonView(locale);

    button.set({
      label: "Adicionar Item",
      icon: '<svg xmlns="http://www.w3.org/2000/svg" fill="#000000" width="800px" height="800px" viewBox="0 0 24 24"><path d="M19 15v-3h-2v3h-3v2h3v3h2v-3h3v-2h-.937zM4 7h11v2H4zm0 4h11v2H4zm0 4h8v2H4z"/></svg>',
      tooltip: true,
    });

    // Mostrar modal cuando se haga clic en el botón

    button.on("execute", () => {
      const html = desciption.value;
      let isAdd = false;
      const pro = html.split("</p>");
      const stringArray = [];

      pro.reverse().forEach((element) => {
        if (
          element.indexOf('<span class="restricted-editing-exception">') > 0 &&
          isAdd == false
        ) {
          const [cantida] = element.split(".");
          const numeroActual = cantida.replace("<p>", "");
          const increment = Number(numeroActual) + 1;

          stringArray.push(
            `<p>${increment}. <span class="restricted-editing-exception">Remplace el texto</span>`
          );
          isAdd = true;
        }
        stringArray.push(element);
      });

      const contents = stringArray.reverse().map((item) => `${item}</p>`);

      editor.model.change((writer) => {
        editorRef.value.data.set(contents.join(""));
      });
    });

    return button;
  });
}

function BloquearPlugin(editor) {

  // editor.ui.componentFactory.add("verPdf", (locale) => {
  //   const button = new ButtonView(locale);

  //   button.set({
  //     label: "Ver PDF",
  //     icon: `<svg height="800px" width="800px" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 309.267 309.267" xml:space="preserve"> <g> <path style="fill:#E2574C;" d="M38.658,0h164.23l87.049,86.711v203.227c0,10.679-8.659,19.329-19.329,19.329H38.658 c-10.67,0-19.329-8.65-19.329-19.329V19.329C19.329,8.65,27.989,0,38.658,0z"/> <path style="fill:#B53629;" d="M289.658,86.981h-67.372c-10.67,0-19.329-8.659-19.329-19.329V0.193L289.658,86.981z"/> <path style="fill:#FFFFFF;" d="M217.434,146.544c3.238,0,4.823-2.822,4.823-5.557c0-2.832-1.653-5.567-4.823-5.567h-18.44 c-3.605,0-5.615,2.986-5.615,6.282v45.317c0,4.04,2.3,6.282,5.412,6.282c3.093,0,5.403-2.242,5.403-6.282v-12.438h11.153 c3.46,0,5.19-2.832,5.19-5.644c0-2.754-1.73-5.49-5.19-5.49h-11.153v-16.903C204.194,146.544,217.434,146.544,217.434,146.544z M155.107,135.42h-13.492c-3.663,0-6.263,2.513-6.263,6.243v45.395c0,4.629,3.74,6.079,6.417,6.079h14.159 c16.758,0,27.824-11.027,27.824-28.047C183.743,147.095,173.325,135.42,155.107,135.42z M155.755,181.946h-8.225v-35.334h7.413 c11.221,0,16.101,7.529,16.101,17.918C171.044,174.253,166.25,181.946,155.755,181.946z M106.33,135.42H92.964 c-3.779,0-5.886,2.493-5.886,6.282v45.317c0,4.04,2.416,6.282,5.663,6.282s5.663-2.242,5.663-6.282v-13.231h8.379 c10.341,0,18.875-7.326,18.875-19.107C125.659,143.152,117.425,135.42,106.33,135.42z M106.108,163.158h-7.703v-17.097h7.703 c4.755,0,7.78,3.711,7.78,8.553C113.878,159.447,110.863,163.158,106.108,163.158z"/> </g></svg>`,
  //     tooltip: true,
  //   });

  //   button.on("execute", async () => {
  //     const pdfBlob = await generatePdf();
  //     const blobUrl = URL.createObjectURL(pdfBlob);

  //     pdfDocument.value = blobUrl;

  //     showPdfViewer.value = true;
  //   });

  //   return button;
  // });
}

function FontSizePlugin(editor) {
  editor.ui.componentFactory.add("tamañoFuente", (locale) => {
    const button = new ButtonView(locale);

    button.set({
      label: "Tamaño Fuente",
      icon: `<svg version="1.0" xmlns="http://www.w3.org/2000/svg"
          width="474.000000pt" height="474.000000pt" viewBox="0 0 474.000000 474.000000"
          preserveAspectRatio="xMidYMid meet">
          <metadata>
          Created by potrace 1.16, written by Peter Selinger 2001-2019
          </metadata>
          <g transform="translate(0.000000,474.000000) scale(0.100000,-0.100000)"
          fill="#000000" stroke="none">
          <path d="M3633 4006 c-200 -207 -363 -381 -363 -386 0 -6 84 -10 215 -10 l215
          0 0 -445 0 -445 295 0 295 0 0 445 0 445 216 0 c128 0 214 4 212 9 -1 5 -138
          150 -303 321 -165 172 -327 342 -360 377 l-60 65 -362 -376z"/>
          <path d="M1121 4188 c-29 -101 -105 -358 -168 -573 -62 -214 -150 -514 -194
          -665 -44 -151 -102 -349 -129 -440 -27 -91 -81 -275 -120 -410 -39 -135 -98
          -335 -130 -445 -101 -343 -235 -801 -300 -1025 -35 -118 -67 -217 -72 -218 -4
          -2 -8 -15 -8 -28 l0 -24 343 0 343 0 56 193 c31 105 97 334 148 507 50 173 95
          326 99 340 l8 25 485 3 486 2 25 -82 c14 -46 66 -229 117 -408 50 -179 108
          -382 128 -452 l36 -128 342 0 342 0 -20 73 c-11 39 -65 227 -119 417 -55 190
          -275 962 -490 1715 -215 754 -420 1467 -454 1585 l-62 215 -320 3 -319 2 -53
          -182z m428 -1273 c28 -99 95 -337 149 -528 55 -191 96 -349 93 -351 -3 -1
          -143 -4 -310 -5 l-303 -2 6 34 c4 19 51 185 105 370 54 184 120 411 147 505
          27 94 52 168 56 164 4 -4 30 -88 57 -187z"/>
          <path d="M3710 1575 l0 -445 -182 -1 c-101 -1 -198 0 -215 1 -18 1 -33 -3 -33
          -9 0 -6 163 -180 362 -387 l362 -376 50 54 c70 73 470 491 589 613 53 55 97
          101 97 102 0 2 -99 3 -220 3 l-220 0 0 445 0 445 -295 0 -295 0 0 -445z"/>
          </g>
          </svg>`,
      tooltip: true,
    });

    // Mostrar modal cuando se haga clic en el botón
    button.on("execute", () => {
      showFontSizeModal.value = true; // Mostrar el modal
    });

    return button;
  });
}

function addItemToList(existingHtml: string): string {
  // Buscar las coincidencias de la numeración en la lista
  const regex =
    /<p>(\d+)\.<span class="restricted-editing-exception">Remplace el texto<\/span><\/p>/g;
  const matches: RegExpExecArray[] = [];
  let match;

  // Ejecutar el regex y almacenar las coincidencias
  while ((match = regex.exec(existingHtml)) !== null) {
    matches.push(match);
  }

  // Determinar el próximo número en la secuencia
  const nextNumber =
    matches.length > 0 ? parseInt(matches[matches.length - 1][1]) + 1 : 1;

  // Crear el nuevo ítem con el siguiente número
  const newItem = `<p>${nextNumber}.<span class="restricted-editing-exception">Remplace el texto</span></p>`;

  // Agregar el nuevo ítem al final del HTML
  const updatedHtml = existingHtml.replace(/(<\/p>)\s*$/, `\n${newItem}\n$&`);

  return updatedHtml;
}

const formatCopia = (copia: string, index: number) => {
  return `Copia ${String(index + 1)}: ${copia}`;
};

// const generatePdf = async () => {
//   let header = "";
//   let asunto = "";
//   let isActive = false;
//   let generado: string;

//   console.log(props.data);
//   const textAfterFirma = `
//         ${props.data?.firmaAprobador}
//         ${props.data?.cargoAprobador}
//         ${props.data?.dependenciaAprobador}
//       `;

//   let newdocuemnt;
//   const pdfm = new PdfManipulate();
//   //text remplace plantilla
//   if (!!props.restric) {
//     const footer = `
//     Anexo: ${props?.data?.anexos}
//     ${
//       Array.isArray(props.data?.copia)
//         ? props.data?.copia
//             .map((copia: string, index: number) => formatCopia(copia, index))
//             .join("\n")
//         : `Copia: ${props.data?.copia}`
//     }
//     Elaboró: ${props.data?.elaboro}
//     Revisó: ${props.data?.reviso ? `Revisó: ${props.data?.reviso}`: "" }
//     Aprobador: ${props.data?.aprobo}
//     Forma de envío: ${props.data?.formaEnvio}
//   `;
//     let paragraphs = desciption.value
//       .split("<p>")
//       .map((item: string) =>
//         item.includes("</p>") ? item.replace("</p>", "") : item
//       )
//       .filter((f) => !f.includes("&nbsp;"));
//     const last = paragraphs[paragraphs.length - 1];
//     if (last === "a") {
//       paragraphs.pop();
//     }

//     paragraphs = paragraphs.map((item) =>
//       item.replace(/<span class="restricted-editing-exception">|<\/span>/g, "")
//     );
//     generado = await pdfm.replaceTextInPdf(
//       props.base64,
//       paragraphs,
//       props.imgBase64
//     );

//     newdocuemnt = await pdfm.addFooterBeforeLastContent(
//       generado,
//       footer,
//       textAfterFirma,
//       isActive,
//       header,
//       asunto
//     );
//   } else {
//     const { imgBase64 } = props;
//     const isImage = imgBase64?.fileType?.includes("image") ?? false;
//     const datatest = `
//   ${desciption.value}

//   <br/>
//    <p>Cordialmente,</p> <br/>
//     ${
//       isImage
//         ? `<img style="aspect-ratio:1365/765;" src="data:image/png;base64,${imgBase64?.fileData}" width="150" height="100">`
//         : imgBase64?.fileName ?? ""
//     }
//    <b><p>${props.data?.firmaAprobador}</p></b>
//    <p>${props.data?.cargoAprobador}</p>
//     <p>${props.data?.dependenciaAprobador}</p>

//   <br/>
//     <p>Anexo: ${props?.data?.anexos} </p>
//     ${
//       Array.isArray(props.data?.copia)
//         ? props.data?.copia
//             .map(
//               (copia: string, index: number) =>
//                 `<p>${formatCopia(copia, index)}</p>`
//             )
//             .join("")
//         : `<p>Copia: ${props.data?.copia ?? ""}</p>`
//     }
//     <p>Elaboró: ${props.data?.elaboro ?? ""}</p>
//     ${props.data?.reviso ? `<p>Reviso: ${props.data?.reviso}</p>`: ""}
//     <p>Aprobador: ${props.data?.aprobo ?? ""}</p>
//     <p>Forma de envío: ${props.data?.formaEnvio ?? ""}</p>
//     <br/>
//     <br/>

//     `;
//     newdocuemnt = await pdfm.generatePdf(
//       datatest,
//       props.base64,
//       props.imgBase64
//     );
//   }

//   const bits = pdfm.base64ToUint8Array(newdocuemnt);

//   const blob = new Blob([bits], { type: "application/pdf" });
//   return blob;
// };

const cleanText = (value) => value.replace(/\s+/g, " ").trim();

const obtenerTextoEtiqueta = (paragraphs: Array<string>) => {
  const textParagraphs = [];

  for (let i = 0; i < paragraphs.length; i++) {
    let paragraphText = paragraphs[i];

    const text = paragraphText.toLocaleLowerCase();
    if (paragraphText.trim()) {
      if (!text.includes("{%firma%}")) {
        paragraphText = cleanText(paragraphText);
        paragraphText = paragraphText.replace(
          "{%des%}",
          '<span class="restricted-editing-exception">Remplace el texto</span>'
        );
        paragraphText = paragraphText.replace("{ { imagen } }", "_sticker_");
      } else {
        paragraphText = paragraphText
          .toLocaleLowerCase()
          .replace("{%firma%}", "_firmaUsuario_");
        paragraphText = paragraphText.replace("{ { imagen } }", "_sticker_");
      }

    }
    textParagraphs.push(`<p>${paragraphText}</p>`);
  }

  const textRemplace = remplaceInternalData(props, textParagraphs);
  const datar = textRemplace.join(" ");

  return datar;
};
const getRandomColor = () => {
  const randomValue = () => Math.floor(Math.random() * 128 + 127); // Valores claros (127 a 255)
  const r = randomValue().toString(16).padStart(2, "0");
  const g = randomValue().toString(16).padStart(2, "0");
  const b = randomValue().toString(16).padStart(2, "0");

  const alpha = Math.floor(0.3 * 255)
    .toString(16)
    .padStart(2, "0"); // 30% de opacidad
  return `#${r}${g}${b}${alpha}`;
};
const escapeHtml = (str) => {
  if (!str) return "";
  return str
    .replace(/&/g, "&amp;")
    .replace(/</g, "&lt;")
    .replace(/>/g, "&gt;")
    .replace(/"/g, "&quot;")
    .replace(/'/g, "&#039;");
};

// Función opcional para sanitizar contenido HTML
const sanitizeHtml = (html) => {
  // Implementar una librería como DOMPurify si es necesario
  // return DOMPurify.sanitize(html);
  return html; // Suponiendo que el contenido es seguro
};
const getDataClonacion = () => {
  let sanitizedHtmlContent = "";

  filteredCollaborators.value.forEach((doc) => {
    // Generar un color aleatorio para el fondo
    const backgroundColor = getRandomColor();
    // Si el id del aporte del colaborador fue aceptado, muestra solo el texto a guardar
    if (acceptedId.value && acceptedId.value === doc.id) {
      sanitizedHtmlContent += `
        <p>${sanitizeHtml(doc.htmlContent)}</p>
      `;
    } else {
      // Mostrar en un div el aporte del colaborador a validar (No se guarda en doc)
      sanitizedHtmlContent += `
      <div id="${doc.id}" style="background-color: ${backgroundColor}; padding: 10px; margin-bottom: 10px; border-radius: 5px;">
        <div>
          <p><strong>${escapeHtml(doc.nombreUsuarioCreador)}</strong></p>
          <div>${sanitizeHtml(doc.htmlContent)}</div>
        </div>
      </div>
      `
    }
  });

  if (acceptedId.value) {
    const index = filteredCollaborators.value.findIndex((item) => item.id === acceptedId.value);
    // // Se elimina el aporte del prop
    filteredCollaborators.value.splice(index, 1);
    acceptedId.value = null;
  }

  return sanitizedHtmlContent;
}

const deleteSelectedDiv = (item: any) => {
  const index = filteredCollaborators.value.findIndex((i) => i.id === item.id);
  // Se elimina el aporte del prop
  filteredCollaborators.value.splice(index, 1);
  if (item.moduloActual && item.moduloActual === "ENTES") {
    // Se procesa párrafo de Entes de Control
    rejectParagraphEntes(item);
  } else {
    // Entonces por defecto es PQRD
    processSelectedDiv(item.userId);
  }
  desciption.value = generateRefValue();
};

const acceptSelectedDiv = (item: any) => {
  acceptedId.value = item.id;
  if (item.moduloActual && item.moduloActual === "ENTES") {
    // Se procesa párrafo de Entes de Control
    acceptParagraphEntes(item);
  } else {
    // Entonces por defecto es PQRD
    processSelectedDiv(item.userId);
  }

  desciption.value = generateRefValue();
}

const processSelectedDiv = async (userId: number) => {
  const response = await sgdeaAxios.patch(
    `/pqrd/desactivarDocumentosSalidaAsociadosAunPqrdClonado?idPqrd=${route.params.id}&idUser=${userId}`
  );
}

const rejectParagraphEntes = async (item: any) => {
  const response = await sgdeaAxios.patch(
    `/api/entes/rechazar-parrafo-doc-salida/${item.id}`,
    {
      motivo_rechazo: `Rechazado por el consolidador ${item?.nombreUsuarioCreador}`,
    }
  );
}

const acceptParagraphEntes = async (item: any) => {
  const response = await sgdeaAxios.patch(
    `/api/entes/aceptar-parrafo-doc-salida/${item.id}`
  );
}

const generateRefValue = () => {
  const header = `
        <span id="header">
        <p>Señor(a): <br/>${props.referentKey.NombredeDestinatario ?? ""}</p>
        ${
    props.referentKey.DireccionDeDestinatario
      ? `<p>Dirección: ${props.referentKey.DireccionDeDestinatario ?? ""}</p>`
      : ""
  }
        ${
    props.referentKey.TelefonoDeDestinatario
      ? `<p>Telefono: ${props.referentKey.TelefonoDeDestinatario ?? ""}</p>`
      : ""
  }
        ${
    props.referentKey.CorrreoDestinatario
      ? `<p>Correo: ${props.referentKey.CorrreoDestinatario ?? ""}</p>`
      : ""
  }
        <p>${props.referentKey.DepartamentoYMunicipioDestinatario ?? ""}</p>
        <p><br/><b>Asunto: ${props.referentKey.asunto ?? ""}</b></p>
        </span>
        <br/>`;
  // Si está vacio el documento, se genera nuevamente la cabecera del documento
  // Si hay colaboraciones las agrega, remueve los divs pintados previamente
  return `${desciption.value && desciption.value.trim().length > 0
            ? desciption.value.replace(/<div[^>]*>[\s\S]*?<\/div>/g, "") : header}
          ${props.collaboration ? getDataClonacion() : ""}
        `;
}

const onReady = async (editorInstance: any) => {
  const toolbarElement = editorToolbarElement.value;
  const menuBarElement = editorMenuBarElement.value;

  if (toolbarElement && editorInstance.ui.view.toolbar) {
    toolbarElement.appendChild(editorInstance.ui.view.toolbar.element);
  }

  if (menuBarElement && editorInstance.ui.view.menuBar) {
    menuBarElement.appendChild(editorInstance.ui.view.menuBar.element);
  }

  try {
    let value = "";

    if (props.restric !== false) {
      const pdfm = await new PdfManipulate();
      const da = await pdfm.extractTextWithPositions(props.base64);
      value = obtenerTextoEtiqueta(da);
    } else {
      value = generateRefValue();
    }

    desciption.value = props.document ? props.document + (props.collaboration ? getDataClonacion(): "") : value;
  } catch {
    desciption.value = "";
  }

  editorInstance.execute("fontFamily", { value: "Arial" });
  editorInstance.execute("fontSize", { value: "14" });
  // editorInstance.execute('justifyFull');

  //editorInstance.model.document.on("change:data", onChange);

  editorRef.value = editorInstance;
};

const eventAutoSave = async (editor) => {

  // let paragraph = "";
  // if (isEdit.value) {
  //   isEdit.value = false;
  //   return Promise.resolve(editor.getData());
  // }
  // // Se eliminan Divs presentes de aportes de clonadores sin confirmar
  // if (props.isCloner && props.collaboration) {
  //   paragraph = editor.getData().replace(/<div[^>]*>[\s\S]*?<\/div>/g, "");
  // } else {
  //   paragraph = editor.getData();
  // }

  // const payload = {
  //   paragraf: paragraph,
  //   pdf: await generatePdf(),
  // };

  // documentStore.setDocumentContent(payload.paragraf);
  // emit("datos-response", payload);
  // isEdit.value = false;

  // return Promise.resolve(editor.getData());
};

const onChange = (_) => {
  isEdit.value = true;
  console.log("change", _);
};

const remplaceInternalData = (
  props: { readonly referentKey?: { [x: string]: string } },
  paragraphs: Array<string> | null
) => {
  const { referentKey = {} } = props;

  // console.log(referentKey, "referentKey++++++");
  if (!paragraphs) {
    return [""];
  }

  const listdata = paragraphs.map((item) => {
    let value = item;

    Object.keys(referentKey).forEach((key) => {
      const val = key.trim();
      const exp = new RegExp(`{%${val}%}`, "g");
      const remplaceData = referentKey[val] || "";

      value = value.replace(exp, remplaceData);
      console.log(value);
    });

    // Reemplazar cualquier {%...%} que no tenga clave en referentKey por una cadena vacía
    value = value.replace(/{%[^%]+%}/g, "");

    return value;
  });

  return listdata;
};

const emitEvent = (event_: number) => (fontSize.value = event_);

watch(
  revisado,
  async (newValue) => {
    // if (newValue === true) {
    //   const docGenerate = await generatePdf();
    //   emit("revisado", docGenerate);
    // }
  },
  { immediate: true }
);

watch(
  aprobado,
  async (newValue) => {
    if (newValue === true) {
      // const docGenerate = await generatePdf();
      // emit("aprobado", docGenerate);
      // console.log(docGenerate.arrayBuffer, "docgenerate");
    }
  },
  { immediate: true }
);

watch(
  fontSize,
  async (value) => {
    if (editorRef.value !== undefined) {
      editorRef.value.execute("fontSize", { value: `${value}` });
    }
  },
  { immediate: true }
);

const classSpan = () => props.collaboration ? "tw-col-span-3" : "tw-col-span-4";

const handlerClick = (item: any) => {
  acceptSelectedDiv(item)
};

const handlerCancel = (item: any) => {
  deleteSelectedDiv(item)
}

</script>

<template>
  <div ref="editorToolbarElement"></div>
  <div class="tw-grid tw-grid-flow-row-dense tw-grid-cols-4 tw-grid-rows-1">
    <div :class="classSpan()">
      <!-- Toolbar personalizada -->
      <ckeditor
        v-model="desciption"
        :editor="DecoupledEditor"
        :config="config"
        @ready="onReady"
        @onkeypress="(vk) => console.log(vk)"
        @onkeydown="(kv) => console.log('dow', kv)"
      />
    </div>

    <div v-if="props.collaboration" class="tw-bg-slate-300 tw-pt-5">
      <div
        v-if="filteredCollaborators.length === 0"
        class="tw-ml-4 tw-mr-4 tw-text-center tw-max-w"
      >
        <p class="tw-inline-block tw-align-baseline tw-font-medium">
          No hay cambios en el documento.
        </p>
      </div>
      <collaboration
        v-for="item,i in filteredCollaborators"
        :key="item.name"
        :userName="item.nombreUsuarioCreador"
        @confir="acceptSelectedDiv(item)"
        @remove="deleteSelectedDiv(item)"
      ></collaboration>
    </div>

    <!-- <div ref="pdfContent" v-html="desciption"></div> -->
    <!-- Menu bar personalizada -->
  </div>
  <FontSizeModal
    @update:showModalSize="showFontSizeModal = false"
    @update:Size="emitEvent"
    v-model="showFontSizeModal"
  />
  <!-- <PdfView
    :showDialog="showPdfViewer"
    :pdfUrl="pdfDocument"
    @update:showDialog="showPdfViewer = $event"
  /> -->
</template>

<style scoped>
.ck-editor__editable {
  min-height: 200px;
  padding: 2cm;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  margin: 0 auto;
  width: 21cm;
  height: 29.7cm;
  overflow: auto;
  border: 1px solid #ddd;
  font-family: Arial, Helvetica, sans-serif;
}

p {
  margin-bottom: 1em; /* Asegura espacio entre los párrafos */
  white-space: normal !important; /* Asegura que los saltos de línea sean respetados */
}

/* Ocultar el texto de "Powered by" cuando el editor está enfocado */
.ck-powered-by {
  visibility: visible;
  transition: visibility 0.3s ease-in-out;
}

/* Cuando el editor está enfocado (condición), oculta el mensaje */
.ck-editor__editable:focus + .ck-powered-by,
.ck-editor__editable:hover + .ck-powered-by {
  visibility: hidden;
}

</style>
